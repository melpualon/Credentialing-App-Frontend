<template>
  <div class="home pt-20">
    <div class="container">
      <div class="flex justify-between py-2 mt-10 border-b border-gray-500">
        <h1 class="text-2xl font-bold text-gray-800 mb-6">Credentialing App API</h1>
        <div class="flex">
          <button @click="updateCertificate()" class="btn-main bg-green-500 mr-5">Update Certificate</button>
          <button @click="addCertificate" class=" bg-blue-500 btn-main">Add Certificate</button>
        </div>
      </div>

      <div class="flex items-center mt-5 mb-10">
        <!-- <input type="text" v-model="id" class="input-main mr-4" placeholder="Certificate Name"> -->
        <input type="text" v-model="certificate" class="input-main mr-4" placeholder="Certificate">
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
        <tr class="bg-white even:bg-gray-400" v-for="certificate in certificates" :key="certificate.id">
          <td class="py-3 font-bold">{{ certificate.certificate }}</td>
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
  data() {
    return {
      certificate: '',
      name: '',
      course: '',
      updatedCert: null,
      certificates: []
    }
  },
  mounted() {
    const axios = require('axios').default;
    this.fetchCertificatesApi()
  },
  methods: {
    addCertificate() {
      if(!this.name || !this.course) {
        alert('Please fill in all fields')
        return
      }
      // const newCertificate = {
      //   id: this.certificates.length + 1,
      //   name: this.name,
      //   course: this.course,
      //   certificate: this.certificate,
      // }

      // console.log(newCertificate)
      axios({
        method: 'post',
        url: 'https://d1lsxracz9.execute-api.ap-southeast-1.amazonaws.com/Stage/api/v1/certificates',
        origin: 'https://d1lsxracz9.execute-api.ap-southeast-1.amazonaws.com/Stage/api/v1/certificates',
        withCredentials: true,
        data: {
          id: this.certificates.length + 1,
          name: this.name,
          course: this.course,
          certificate: this.certificate
        }
      })
      .then(res => console.log(res))
      .catch(err => console.log(err))


      
      // axios.post('https://d1lsxracz9.execute-api.ap-southeast-1.amazonaws.com/Stage/api/v1/certificates', newCertificate)
      // .then(res => res.header("Access-Control-Allow-Origin", "*"))
      // .catch(err => console.log(err))
      // this.certificates = [...this.certificates, this.newCertificate]
      
      this.name = '',
      this.course = ''
    },
    deleteCertificate(id) {
      // this.certificates = this.certificates.filter(item => item.id !== id)

      axios.delete(`https://d1lsxracz9.execute-api.ap-southeast-1.amazonaws.com/Stage/api/v1/certificates/${id}`)
      .then(res => console.log(res))
      .catch(err => console.log(err))
    },
    editCertificate(id) {
      // console.log(id)
      const filtered = this.certificates.filter(item => item.id === id)
      this.certificate = filtered[0].certificate
      this.name = filtered[0].name
      this.course = filtered[0].course

      // console.log(filtered[0])
    },

    updateCertificate() {
      this.updatedCert = {
        certificate: this.certificate,
        name: this.name,
        course: this.course,
        isEditing: false
      }

      console.log(this.updatedCert)
      // this.certificates[0]

      // this.certificates[0] = this.updatedCert

      // this.certificates.find(item => {
      //   item.isEditing === true
      // })

      // const findCert = this.certificates.indexOf(this.certificates.find(item => item.isEditing === true))
      // console.log(findCert)
      // this.certificates.splice(findCert, 1, this.updatedCert)
     
    

      // console.log(findCert)
      // this.certificates = [...this.certificates, this.updatedCert]

      this.certificate = ''
      this.name = ''
      this.course = ''
      this.isEditing = false
    },

    async fetchCertificatesApi() {
      axios({
        method: 'get',
        url: 'https://d1lsxracz9.execute-api.ap-southeast-1.amazonaws.com/Stage/api/v1/certificates',
        origin: 'https://d1lsxracz9.execute-api.ap-southeast-1.amazonaws.com/Stage/api/v1/certificates',
        withCredentials: true,
      })
      .then(res => console.log(res))
      .catch(err => console.log(err))

      // const res = await fetch('https://d1lsxracz9.execute-api.ap-southeast-1.amazonaws.com/Stage/api/v1/certificates')
      // // const res = await fetch('http://localhost:3000/certificates')
      // const data = await res.json()
      // this.certificates = data.data
    }
  }
}
</script>
