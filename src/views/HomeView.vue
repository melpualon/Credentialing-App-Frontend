<template>
  <div class="home pt-20">
    <div class="container">
      <div class="flex justify-between py-2 mt-10 border-b border-gray-500">
        <h1 class="text-2xl font-bold text-gray-800 mb-6">Credentialing App API</h1>
        <div class="flex">
          <button @click="addCertificate" class=" bg-blue-500 btn-main">Add Certificate</button>
        </div>
      </div>

      <div class="flex items-center mt-5 mb-10">
        <input type="text" v-model="certName" class="input-main mr-4" placeholder="Certificate Name">
        <input type="text" v-model="owner" class="input-main mr-4" placeholder="Name of Owner">
        <input type="text" v-model="course" class="input-main" placeholder="Course">
      </div>
    </div>

    <div class="container mt-16">
      <table id="table"  class="w-full">
        <tr class="text-left border-b border-gray-300 !bg-white">
          <th class="py-4">Certificate</th>
          <th class="py-4">Name</th>
          <th class="py-4">Course</th>
          <th class="py-4"></th>
          <th class="py-4"></th>
        </tr>
        <tr class="bg-white even:bg-gray-400" v-for="certificate in certificates" :key="certificate.id">
          <td class="py-3 font-bold">{{ certificate.certName }}</td>
          <td class="py-3">{{ certificate.owner }}</td>
          <td class="py-3">{{ certificate.course }}</td>
          <td class="py-3"><button @click="editCertificate(certificate.id)" class="font-bold text-green-600">Edit</button></td>
          <td class="py-3"><button class="font-bold text-red-600" @click="deleteCertificate(certificate.id)">Delete</button></td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  components: {},
  data() {
    return {
      certName: '',
      owner: '',
      course: '',
      certificates: []
    }
  },
  mounted() {
    // fetch('http://localhost:3000/certificates')
    // .then(res => res.json())
    // .then(data => this.certificates = data)
    // .catch(err => console.log(err.message))
    this.fetchCertificatesApi()
  },
  methods: {
    deleteCertificate(id) {
      this.certificates = this.certificates.filter(item => item.id !== id)
    },
    addCertificate() {
      const { certName , owner, course } = this

      if(!certName || !owner || !course) {
        alert('Please fill in all fields')
        return
      }

      this.certificates.push({
        certName: certName,
        owner: owner,
        course: course,
        id: this.certificates.length + 1
      })

      this.certName = ''
      this.owner = '',
      this.course = ''
    },
    editCertificate() {
      console.log('ceritifcate is edited')
    },

    async fetchCertificatesApi() {
      const res = await fetch('http://localhost:3000/certificates')
      const data = await res.json()
      
      this.certificates = data
    }
  }
}
</script>
