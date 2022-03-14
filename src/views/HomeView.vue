<template>
  <div class="home">
    <h1>Credentialing App API</h1>
    <div>
      <input type="text" v-model="name">
      <input type="text" v-model="course">
      <button @click="addCertificate">Add Certificate</button>
    </div>

    <div v-for="certificate in certificates" :key="certificate.id">
      <h2>Name: {{ certificate.name }}</h2>
      <p>Course: {{ certificate.course }}</p>
      <button @click="deleteCertificate(certificate.id)">Delete Certificate</button>
    </div>
  </div>
</template>

<script>


export default {
  components: {},
  data() {
    return {
      name: '',
      course: '',
      certificates: [
        { name: 'Mel Doe', course: 'Engineering', id: 1 },
        { name: 'Jane Fox', course: 'Accountancy', id: 2 },
        { name: 'Chris Don', course: 'Civil Engineer', id: 3 },
      ]
    }
  },
  methods: {
    deleteCertificate(id) {
      this.certificates = this.certificates.filter(item => item.id !== id)
    },
    addCertificate() {
      if(!this.name || !this.course) {
        console.log('Please fill in all fields')
        return
      }

      this.certificates.push({
        name: this.name,
        course: this.course,
        id: this.certificates.length + 1
      })
    }
  }
}
</script>
