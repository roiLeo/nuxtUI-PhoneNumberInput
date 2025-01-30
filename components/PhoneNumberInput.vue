<template>
  <UFormGroup
    :error="modelValue && !results.isValid"
    label="Phone number"
    name="phone"
  >
    <UButtonGroup class="w-full">
      <USelectMenu
        class="w-40"
        v-model="selectedCountry"
        clear-search-on-close
        searchable
        searchable-placeholder="Search a country..."
        :options="countries"
        value-attribute="id"
      >
        <template #leading>
          <span
            class="relative inline-flex h-5 w-5 flex-shrink-0 items-center justify-center rounded-full text-[10px]"
          >
            <span class="grey-text text-xs">+{{ selectedCountryDialcode }}</span>
          </span>
        </template>
        <template #option="{ option: country }">
          <span
            class="relative inline-flex h-5 w-5 flex-shrink-0 items-center justify-center rounded-full text-[10px]"
          >
            <span class="grey-text text-xs">+{{ country.dialCode }}</span>
          </span>
          <div class="inline truncate">
            {{ country.label }}
          </div>
        </template>
      </USelectMenu>
      <UInput
        type="tel"
        class="w-full"
        :trailing-icon="
          modelValue && !results.isValid
            ? 'i-heroicons-exclamation-triangle-20-solid'
            : 'i-heroicons-check-20-solid'
        "
        :model-value="phoneNumber"
        :placeholder="inputLabel"
        @update:model-value="
          onPhoneNumberChanged({
            newPhoneNumber: $event,
            autoFormat,
            noFormattingAsYouType: false,
          })
        "
      />
    </UButtonGroup>
  </UFormGroup>
</template>

<script lang="ts" setup>
import {
  type CountryCode,
  type Examples,
  getCountries,
  getCountryCallingCode,
} from 'libphonenumber-js'

defineProps({ modelValue: { type: null, required: true } })
const emit = defineEmits(['update:modelValue'])
const {
  getPhoneNumberExamplesFile,
  getPhoneNumberExample,
  getPhoneNumberResults,
  sanitizePhoneNumber,
} = usePhonenumber()

const locale = ref('en')
const phoneNumber = ref<string>('')
const autoFormat = ref<boolean>(true)
const selectedCountry = ref<CountryCode>('FR')
const results = ref<any>({
  isValid: false,
  countryCode: undefined,
})

const selectedCountryDialcode = computed(() => countries.value?.filter((country) => country.id === selectedCountry.value)[0]?.dialCode)
const inputLabel = computed(() => {
  const defaultPlaceholder = 'example'
  const example = getPhoneNumberExample(examples.value as Examples, selectedCountry.value)
  return !example ? defaultPlaceholder : example
})

let displayNamesInstance: Intl.DisplayNames | undefined = undefined
let displayNamesLocale: string | undefined = undefined

const getCountryName = (
  locale: string,
  code: CountryCode | string,
  customCountriesNameListByIsoCode?: Record<CountryCode, string>
): string | undefined => {
  if (customCountriesNameListByIsoCode?.[code]) {
    return customCountriesNameListByIsoCode[code]
  }

  if (displayNamesLocale !== locale || !displayNamesInstance) {
    displayNamesLocale = locale
    displayNamesInstance = new Intl.DisplayNames([locale], { type: 'region' })
  }

  return displayNamesInstance.of(code)
}

const { data: examples } = await useAsyncData(async () => await getPhoneNumberExamplesFile())

const { data: countries } = await useAsyncData(async () => {
  const isoList = getCountries()
  const countriesList: any[] = []
  for (const iso2 of isoList) {
    const label = getCountryName(locale.value, iso2)
    if (label) {
      const dialCode = getCountryCallingCode(iso2)
      countriesList.push({
        id: iso2,
        dialCode,
        label,
      })
    }
  }

  return countriesList
})

watch(
  () => selectedCountry.value,
  (value, oldValue) => {
    if (value && value !== oldValue && value !== selectedCountry.value) {
      onCountryChanged({
        countryCode: value,
        autoFormat: autoFormat.value,
        noFormattingAsYouType: false,
      })
    }
  },
  {
    immediate: true,
  }
)

function onPhoneNumberChanged({
  newPhoneNumber,
  autoFormat,
  noFormattingAsYouType,
  updateResults = true,
}: {
  newPhoneNumber: string
  autoFormat: boolean
  noFormattingAsYouType: boolean
  updateResults?: boolean
}) {
  const containsOnlyNumbers = /^[\d\s]*$/.test(newPhoneNumber)

  if (!containsOnlyNumbers) {
    results.value = { isValid: false, countryCode: undefined }
    emit('update:modelValue', results.value)
    return
  }

  const sanitizedPhoneNumber = sanitizePhoneNumber(newPhoneNumber)

  if (sanitizedPhoneNumber === '') {
    results.value = { isValid: false, countryCode: undefined }
    emit('update:modelValue', null)
  } else if (updateResults) {
    results.value = getPhoneNumberResults({
      phoneNumber: sanitizedPhoneNumber,
      countryCode: selectedCountry.value,
    })
    emit('update:modelValue', results.value)
  }

  if (results.value.isValid && results.value.formatNational && autoFormat) {
    phoneNumber.value = results.value.formatNational
  } else {
    phoneNumber.value = sanitizedPhoneNumber
  }

  if (
    results.value.countryCode &&
    results.value.countryCode !== selectedCountry.value
  ) {
    onCountryChanged({
      countryCode: results.value.countryCode,
      autoFormat,
      noFormattingAsYouType,
      updateResults: false,
    })
  }
}

function onCountryChanged({
  countryCode,
  autoFormat,
  noFormattingAsYouType,
  updateResults = true,
}: {
  countryCode?: CountryCode
  autoFormat: boolean
  noFormattingAsYouType: boolean
  updateResults?: boolean
}) {
  if (updateResults) {
    results.value = getPhoneNumberResults({
      phoneNumber: phoneNumber.value,
      countryCode,
    })
  }

  onPhoneNumberChanged({
    newPhoneNumber: phoneNumber.value,
    autoFormat,
    noFormattingAsYouType,
    updateResults,
  })
}
</script>
