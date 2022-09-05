<template>
  <address-detail :is-edit="false" />
</template>

<script>
import AddressDetail from './components/AddressDetail'

export default {
  name: 'AddressCreate',
  components: { AddressDetail }
}
</script>

