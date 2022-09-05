<template>
  <address-detail :is-edit="true" />
</template>

<script>
import AddressDetail from './components/AddressDetail'

export default {
  name: 'AddressEdit',
  components: { AddressDetail }
}
</script>

