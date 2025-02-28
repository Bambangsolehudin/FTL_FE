<template>
  <div class="flex flex-col h-screen">
    <!-- Header -->
    <header class="bg-gradient-to-r from-gray-900 to-gray-600 text-white flex justify-between items-center p-2 shadow-md">
      <div class="text-xl font-bold">FTL <span class="ml-6">IMeeting</span></div>
      <div class="flex items-center space-x-4">
        <span class="text-sm">John Doe</span>
        <img src="https://via.placeholder.com/40" alt="Profile" class="w-10 h-10 rounded-full">
      </div>
    </header>

    <div class="flex flex-1">
      <!-- Sidebar -->
      <div class="w-1/8 bg-white  p-2 text-white mt-10 shadow-2xl">
        <ul>
          <li class="mb-2"><a href="#" class="text-indigo-950 hover:text-teal-400 text-center">Home</a></li>
          <li class="mb-2"><a href="#" class="text-indigo-950 hover:text-teal-400 text-center">User</a></li>
        </ul>
      </div>
      
      <!-- Main Content -->
      <div class="w-7/8 p-6">
        <div class="flex justify-between items-center mb-4">
          <h1 class="text-2xl font-bold">Ruang Meeting</h1>
          <button class="bg-teal-600 text-white px-4 py-2 rounded-lg" 
            @click.prevent="$router.push('/about')"
          >+ Pesan Ruangan</button>
        </div>
        
        <table class="w-full bg-white shadow-md rounded-lg overflow-hidden">
          <thead class="bg-gray-200">
            <tr>
              <th class="p-3 text-left">UNIT</th>
              <th class="p-3 text-left">RUANG MEETING</th>
              <th class="p-3 text-left">KAPASITAS</th>
              <th class="p-3 text-left">TANGGAL RAPAT</th>
              <th class="p-3 text-left">WAKTU</th>
              <th class="p-3 text-left">JUMLAH PESERTA</th>
              <th class="p-3 text-left">JENIS KONSUMSI</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(meeting, index) in meetings" :key="index" class="">
              <td class="p-4 font-semibold">{{ meeting.unit }}</td>
              <td class="p-4">{{ meeting.unit }}</td>
              <td class="p-4">{{ meeting.ruangMeeting }}</td>
              <td class="p-4">{{ meeting.tanggalRapat }}</td>
              <td class="p-4">{{ meeting.waktuMulai }} s/d {{ meeting.waktuSelesai }}</td>
              <td class="p-4">{{ meeting.jumlahPeserta }}</td>
              <td class="p-4">
                 {{meeting.jenisKonsumsi?.join(',')}}
              </td>
              
            </tr>
          </tbody>
        </table>
        
        <div class="flex justify-end mt-4 space-x-2">
          <button class="px-3 py-1 bg-gray-300 rounded">Previous</button>
          <button class="px-3 py-1 bg-teal-600 text-white rounded">1</button>
          <button class="px-3 py-1 bg-gray-300 rounded">Next</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      meetings: []
    };
  },
  mounted() {
    this.fetchMeetings();
  },
  methods: {
    filteredItems(obj) {
      return Object.fromEntries(
        Object.entries(obj).filter(([_, value]) => value)
      );
    },
    async fetchMeetings() {
      try {
        const response = await axios.get('http://localhost:5000/api/meetings');
        this.meetings = response.data;

        console.log('meetings', this.meetings);
        
      } catch (error) {
        console.error('Error fetching meetings:', error);
      }
    },
    async addMeeting() {
      const newMeeting = {
        unit: "UNIT IT",
        room: "Ruang Borobudur",
        capacity: "15 Orang",
        date: "12 Desember 2024",
        time: "10:00 s/d 12:00",
        participants: "5 Orang",
        consumption: "Snack Pagi"
      };
      try {
        const response = await axios.post('http://localhost:5000/api/meetings', newMeeting);
        this.meetings.push(response.data);
      } catch (error) {
        console.error('Error adding meeting:', error);
      }
    },
    async deleteMeeting(id) {
      try {
        await axios.delete(`http://localhost:5000/api/meetings/${id}`);
        this.meetings = this.meetings.filter(meeting => meeting.id !== id);
      } catch (error) {
        console.error('Error deleting meeting:', error);
      }
    }
  }
};
</script>

<style scoped>
  table {
    border-collapse: collapse;
    width: 100%;
  }
  th, td {
    text-align: left;
    padding: 8px;
  }
</style>
