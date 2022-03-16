<template>
  <div class="home pt-20">
    <div class="container">
      <div class="flex justify-between py-2 mt-10 border-b border-gray-500">
        <h1 class="text-2xl font-bold text-gray-800 mb-6">Credentialing App API</h1>
        <div class="flex">
          <button @click="updateCertificate" class="btn-main bg-green-500 mr-5">Update Certificate</button>
          <button @click="addCertificate" class=" bg-blue-500 btn-main">Add Certificate</button>
        </div>
      </div>

      <div class="flex items-center mt-5 mb-10">
        <!-- <input type="text" v-model="id" class="input-main mr-4" placeholder="Certificate Name"> -->
        <input type="text" v-model="name" class="input-main mr-4" placeholder="Name of Owner">
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
        <tr class="bg-white even:bg-gray-400" v-for="certificate in certificates" :key="certificate.id" :cert-id="certificate.id">
          <td class="py-3 font-bold">{{ certificate.id }}</td>
          <td class="py-3">{{ certificate.name }}</td>
          <td class="py-3">{{ certificate.course }}</td>
          <td class="py-3">
            <button @click="editCertificate(certificate.id)" class="font-bold text-green-600">Edit</button>
          </td>
          <td class="py-3"><button class="font-bold text-red-600" @click="deleteCertificate(certificate.id)">Delete</button></td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  components: {},
  props: [ 'cert-id', 'key' ],
  data() {
    return {
      id: '',
      name: '',
      course: '',
      updatedCert: null,
      newCertificate: null,
      certificates: []
    }
  },
  mounted() {
    this.fetchCertificatesApi()

    const axios = require('axios').default;
  },
  methods: {
    addCertificate() {
      if(!this.name || !this.course) {
        alert('Please fill in all fields')
        return
      }
      this.newCertificate = {
        id: this.certificates.length + 1, 
        name: this.name,
        course: this.course
      }

      axios.post('https://d1lsxracz9.execute-api.ap-southeast-1.amazonaws.com/Stage/api/v1/certificates', this.newCertificate)
      this.certificates = [...this.certificates, this.newCertificate]
      
      this.name = '',
      this.course = ''
    },
    deleteCertificate(id) {
      this.certificates = this.certificates.filter(item => item.id !== id)
    },
    editCertificate(id) {
      console.log(id)
      const filtered = this.certificates.filter(item => item.id === id)

      // console.log(filtered[0].certName)
      this.certName = filtered[0].certName
      this.owner = filtered[0].owner
      this.course = filtered[0].course


    },

    updateCertificate() {
      this.updatedCert = {
        certName: this.certName,
        owner: this.owner,
        course: this.course,
      }

      const getId = this['cert-id']
      console.log(getId)

      // this.certificates = [...this.certificates, this.updatedCert]

      this.certName = '',
      this.owner = '',
      this.course = ''

      console.log(this.updatedCert)
    },

    async fetchCertificatesApi() {
      const res = await fetch('https://d1lsxracz9.execute-api.ap-southeast-1.amazonaws.com/Stage/api/v1/certificates')
      // const res = await fetch('http://localhost:3000/certificates')

      const data = await res.json()
      this.certificates = data.data

      // console.log(data.data)
    }
  }
}
</script>
