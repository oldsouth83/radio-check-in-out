# radio-check-in-out
<!DOCTYPE html>
<html>
<head>
  <title>Radio Sign In/Out</title>
</head>
<body>
  <h2>Radio Check In/Out</h2>
  <form id="radioForm">
    <label for="name">Your Name:</label><br>
    <input type="text" id="name" required><br><br>

    <label for="radioId">Radio ID:</label><br>
    <input type="text" id="radioId" required><br><br>

    <label for="action">Action:</label><br>
    <select id="action">
      <option value="Sign Out">Sign Out</option>
      <option value="Sign In">Sign In</option>
    </select><br><br>

    <button type="submit">Submit</button>
  </form>

  <p id="message" style="color:red;"></p>

  <script>
    const webAppUrl = "(https://script.google.com/macros/s/AKfycbw00YjaHigBkOwJno9-yljk5CmShohxvP7ya951BxeYwX0Es-1ArGHdkTn5yV2bQG_A6A/exec)"; // <-- Replace with your script's Web App URL

    document.getElementById("radioForm").addEventListener("submit", async function(event) {
      event.preventDefault();

      const name = document.getElementById("name").value;
      const radioId = document.getElementById("radioId").value;
      const action = document.getElementById("action").value;

      // Check radio status
      const response = await fetch(`${webAppUrl}?radioId=${encodeURIComponent(radioId)}`);
      const data = await response.json();

      if (action === "Sign Out" && data.status === "Signed Out") {
        document.getElementById("message").textContent = `Warning: ${radioId} is already signed out.`;
        return;
      } else {
        document.getElementById("message").textContent = "";
      }

      // Submit the form data
      const formData = new URLSearchParams();
      formData.append("name", name);
      formData.append("radioId", radioId);
      formData.append("action", action);

      await fetch(webAppUrl, {
        method: "POST",
        body: formData
      });

      alert("Submission successful!");
      document.getElementById("radioForm").reset();
    });
  </script>
</body>
</html>
