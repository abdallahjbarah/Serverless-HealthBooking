updateStatus(appointment, newStatus) {
  const cleanAppointment = JSON.parse(JSON.stringify(appointment));
  const url = `https://2xvatz15uf.execute-api.us-east-1.amazonaws.com/Api/Appointments/${cleanAppointment.appointmentId}`;

  const payload = { status: newStatus };

  fetch(url, {
    method: "PATCH",
    headers: {
      "Content-Type": "application/json"
    },
    body: JSON.stringify(payload)
  })
    .then(async res => {
      const rawBody = await res.text();

      if (!res.ok) {
        throw new Error(`HTTP ${res.status}: ${rawBody}`);
      }

      return JSON.parse(rawBody);
    })
    .then(() => {
      alert("Status updated!");
    })
    .catch(err => {
      console.error("Failed to update status:", err);
      alert("Update failed. See console for details.");
    });
}

