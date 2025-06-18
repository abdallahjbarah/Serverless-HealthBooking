<script>
export default {
  name: "AppointmentsList",
  data() {
    return {
      appointments: []
    };
  },
  mounted() {
    this.fetchAppointments();
  },
  methods: {
    // GET /Api/Appointments
    fetchAppointments() {
      fetch("https://2xvatz15uf.execute-api.us-east-1.amazonaws.com/Api/Appointments")
        .then(res => res.json())
        .then(data => {
          // If your Lambda returns { body: "json‑string" } keep the next line,
          // otherwise just use: this.appointments = data;
          const parsed = JSON.parse(data.body);
          this.appointments = parsed;
        })
        .catch(err => console.error("Failed to load appointments:", err));
    },

    // PATCH /Api/Appointments/{id}
    updateStatus(appointment, newStatus) {
      const url = `https://2xvatz15uf.execute-api.us-east-1.amazonaws.com/Api/Appointments/${appointment.appointmentId}`;
      const payload = { status: newStatus };

      fetch(url, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(payload)
      })
        .then(async res => {
          const rawBody = await res.text();
          if (!res.ok) throw new Error(`HTTP ${res.status}: ${rawBody}`);
          return JSON.parse(rawBody);
        })
        .then(() => {
          // update UI immediately
          appointment.status = newStatus;
          alert("Status updated!");
        })
        .catch(err => {
          console.error("Failed to update status:", err);
          alert("Update failed. See console for details.");
        });
    }
  }
};
</script>
