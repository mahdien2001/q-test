<template>
    <div>
        <q-inner-loading
        :showing="$store.state.loading"
        label="Please wait..."
        label-class="text-secondary"
        label-style="font-size: 1.1em"
        style="z-index: 100;"
      />
    </div>
</template>
