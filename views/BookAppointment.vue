<script>
export default {
  name: "BookAppointment",
  data() {
    return {
      name: "",
      symptoms: "",
      selectedSlot: "",
      slots: []
    };
  },
  mounted() {
    fetch("https://2xvatz15uf.execute-api.us-east-1.amazonaws.com/Api/Slot")
      .then(res => res.json())
      .then(data => {
        const parsed = JSON.parse(data.body);  // if your Lambda returns `{ body: string }`
        this.slots = parsed.filter(s => !s.isBooked).map(s => s.slot);
      })
      .catch(err => {
        console.error("Error fetching slots:", err);
        alert("Failed to load available slots.");
      });
  },
  methods: {
    submitAppointment() {
      const payload = {
        patientName: this.name,
        symptoms: this.symptoms,
        slot: this.selectedSlot
      };

      fetch("https://2xvatz15uf.execute-api.us-east-1.amazonaws.com/Api/Appointments", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(payload) // Remove outer { body: ... } unless needed
      })
        .then(res => res.json())
        .then(() => {
          alert("Appointment booked!");
          this.name = "";
          this.symptoms = "";
          this.selectedSlot = "";
        })
        .catch(err => {
          console.error("Error booking appointment:", err);
          alert("Failed to book appointment.");
        });
    }
  }
};
</script>

