# Nuxt UI Phone Number Input

A production-ready phone number input component for Nuxt 4 with Nuxt UI. Features international phone number validation, formatting, and country selection with real-time validation.

## Features

- 🌍 **International Support** - Supports all countries with automatic dial code detection
- ✅ **Validation** - Real-time phone number validation using libphonenumber-js
- 🎨 **Nuxt UI Integration** - Built on Nuxt UI components for consistent styling
- 🔤 **Auto-Formatting** - Automatic phone number formatting as you type
- 🌐 **Locale Support** - Multi-language country name display
- 📦 **Type Safe** - Full TypeScript support
- 🎯 **Accessible** - Built with accessibility in mind using Nuxt UI form components

## Prerequisites

- Node.js 18+
- pnpm 9.4.0+
- Nuxt 4.2.2+

## Installation

```bash
# Clone the repository
git clone <repository-url>
cd nuxtUI-PhoneNumberInput

# Install dependencies
pnpm install

# Start development server
pnpm dev

# Build for production
pnpm build
```

## Usage

### Basic Example

```vue
<script setup lang="ts">
import { ref } from 'vue'

const phoneNumber = ref('')

const handlePhoneUpdate = (value) => {
  phoneNumber.value = value
}
</script>

<template>
  <PhoneNumberInput v-model="phoneNumber" @update:modelValue="handlePhoneUpdate" />
</template>
```

## Component Props

### `PhoneNumberInput`

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `modelValue` | `string \| null` | required | The phone number value (v-model) |

## Component Features

### Country Selection
- Searchable dropdown with all countries
- Displays country flag with dial code
- Auto-selects based on locale

### Phone Number Input
- Auto-formatting as you type
- Placeholder shows example number for selected country
- Real-time validation feedback
- Icon indicator for valid/invalid state

### Validation Results

The component provides detailed validation results:

- `isValid` - Boolean indicating if the phone number is valid
- `countryCode` - The detected or selected country code
- `nationalNumber` - The phone number without country code
- `formatInternational` - International format (e.g., +1 201-555-0123)
- `formatNational` - National format (e.g., (201) 555-0123)
- `e164` - E.164 format for storage
- `type` - Phone number type (mobile, fixed-line, etc.)
- `isPossible` - Whether the number is possible to exist

## Composable: `usePhonenumber()`

The `usePhoneNumber` composable provides utility functions for phone number handling:

```typescript
const {
  getPhoneNumberExamplesFile,
  getPhoneNumberExample,
  getPhoneNumberResults,
  sanitizePhoneNumber,
} = usePhonenumber()
```

### Methods

- **`getPhoneNumberExamplesFile()`** - Fetches example numbers for all countries
- **`getPhoneNumberExample(examples, countryCode)`** - Gets example number for a specific country
- **`getPhoneNumberResults({ phoneNumber, countryCode })`** - Validates and parses phone number
- **`sanitizePhoneNumber(input)`** - Removes invalid characters from input

## Technologies

- **Nuxt 4** - Vue meta-framework
- **Nuxt UI** - Component library
- **libphonenumber-js** - Phone number parsing and validation
- **TypeScript** - Type safety
- **Tailwind CSS** - Styling

## Development

```bash
# Start dev server with hot reload
pnpm dev

# Build for production
pnpm build

# Generate static site
pnpm generate

# Preview production build
pnpm preview
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.
