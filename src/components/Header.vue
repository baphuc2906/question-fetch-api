<template>
  <div>
    <b-nav tabs>
      <b-nav-item disabled>Fancy Quiz App</b-nav-item>
      <b-nav-item disabled>Couter: {{numCorrect}}/{{numTotal}}</b-nav-item>
    </b-nav>
  </div>
</template>
<script>
  export default {
    // props: {
    //   numCorrect: Number,
    //   numTotal: Number
    // }
    props: [
      'numCorrect',
      'numTotal'
    ]
  }
</script>
