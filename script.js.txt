// 🔥 Firebase Configuration (REPLACE with your own)
var firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_PROJECT.firebaseapp.com",
    databaseURL: "https://YOUR_PROJECT.firebaseio.com",
    projectId: "YOUR_PROJECT",
    storageBucket: "YOUR_PROJECT.appspot.com",
    messagingSenderId: "XXXX",
    appId: "XXXX"
};

firebase.initializeApp(firebaseConfig);
var database = firebase.database();

let selectedEvent = "";

// Open form
function openForm(eventName) {
    selectedEvent = eventName;
    document.getElementById("eventTitle").innerText = eventName;
    document.getElementById("formContainer").style.display = "block";
}

// Close form
function closeForm() {
    document.getElementById("formContainer").style.display = "none";
    document.getElementById("registrationForm").reset();
}

// Submit registration
document.getElementById("registrationForm").addEventListener("submit", function(e) {
    e.preventDefault();

    let name = document.getElementById("name").value;
    let email = document.getElementById("email").value;

    database.ref("registrations").push({
        name: name,
        email: email,
        event: selectedEvent
    });

    alert("Registration successful!");
    closeForm();
});

// Load registrations
function loadRegistrations() {
    const table = document.getElementById("registrationTable");
    const tbody = table.querySelector("tbody");
    tbody.innerHTML = "";

    database.ref("registrations").once("value", function(snapshot) {
        snapshot.forEach(function(child) {
            let data = child.val();
            let row = `
                <tr>
                    <td>${data.name}</td>
                    <td>${data.email}</td>
                    <td>${data.event}</td>
                </tr>`;
            tbody.innerHTML += row;
        });
        table.style.display = "table";
    });
}

