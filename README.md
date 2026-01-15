# LAB07
[Lab07 2.zip](https://github.com/user-attachments/files/24637262/Lab07.2.zip)
[app.js](https://github.com/user-attachments/files/24637451/app.js)
const loadUserBtn = document.getElementById("btnLoad");
const clearBtn = document.getElementById("btnClear");
const statusDiv = document.getElementById("status");
const resultDiv = document.getElementById("result");

loadUserBtn.addEventListener("click", async () => {
  statusDiv.textContent = "Loading...";
  loadUserBtn.disabled = true;

  resultDiv.classList.add("hidden");

  try {
    const res = await fetch("https://randomuser.me/api/");

    if (!res.ok) {
      throw new Error(`HTTP ${res.status}`);
    }

    const data = await res.json();
    const user = data.results[0];

    resultDiv.innerHTML = `
      <img
        src="${user.picture.large}"
        alt="User Avatar"
        class="w-24 h-24 object-cover mb-3"
      />
      <p class="font-semibold">
        ${user.name.first} ${user.name.last}
      </p>
      <p class="text-gray-600 text-sm">
        ${user.email}
      </p>
    `;

    resultDiv.classList.remove("hidden");
    statusDiv.textContent = "Loaded successfully.";
  } catch (err) {
    statusDiv.textContent = `Error: ${err.message}`;
  } finally {
    loadUserBtn.disabled = false;
  }
});

clearBtn.addEventListener("click", () => {
  statusDiv.textContent = "";
  resultDiv.classList.add("hidden");
  resultDiv.innerHTML = "";
});
