<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Advanced Contact Manager</title>

<style>
*{
margin:0;
padding:0;
box-sizing:border-box;
font-family:'Segoe UI',sans-serif;
}

body{
background:linear-gradient(135deg,#0f2027,#203a43,#2c5364);
min-height:100vh;
padding:30px;
color:white;
}

.container{
display:grid;
grid-template-columns:350px 1fr;
gap:25px;
}

.card{
background:rgba(255,255,255,0.1);
backdrop-filter:blur(10px);
padding:25px;
border-radius:20px;
box-shadow:0 8px 32px rgba(0,0,0,0.3);
}

h2{
margin-bottom:20px;
text-align:center;
}

input{
width:100%;
padding:12px;
margin:10px 0;
border:none;
border-radius:10px;
outline:none;
}

button{
width:100%;
padding:12px;
background:#00c6ff;
color:white;
border:none;
border-radius:10px;
cursor:pointer;
font-size:16px;
transition:.3s;
}

button:hover{
background:#0072ff;
transform:scale(1.03);
}

.error{
color:#ff6b6b;
font-size:14px;
margin-top:5px;
}

.search{
margin-bottom:20px;
}

.contacts{
display:grid;
grid-template-columns:repeat(auto-fill,minmax(250px,1fr));
gap:20px;
}

.contact-card{
background:rgba(255,255,255,0.12);
padding:20px;
border-radius:15px;
animation:fadeIn .5s;
}

.contact-card h3{
margin-bottom:10px;
}

.contact-card p{
margin:5px 0;
}

.delete-btn{
margin-top:10px;
background:#ff4d4d;
}

.delete-btn:hover{
background:#cc0000;
}

@keyframes fadeIn{
from{
opacity:0;
transform:translateY(20px);
}
to{
opacity:1;
transform:translateY(0);
}
}

@media(max-width:768px){

.container{
grid-template-columns:1fr;
}

}
</style>
</head>
<body>

<div class="container">

<div class="card">

<h2>Contact Form</h2>

<form id="contactForm">

<input type="text" id="name" placeholder="Full Name">

<input type="email" id="email" placeholder="Email">

<input type="text" id="phone" placeholder="Phone Number">

<button type="submit">
Add Contact
</button>

<div class="error" id="error"></div>

</form>

</div>

<div class="card">

<h2>Contact Manager</h2>

<input
type="text"
id="search"
class="search"
placeholder="Search Contact..."
>

<div class="contacts" id="contactList"></div>

</div>

</div>

<script>

const form = document.getElementById("contactForm");
const contactList = document.getElementById("contactList");
const error = document.getElementById("error");
const search = document.getElementById("search");

let contacts =
JSON.parse(localStorage.getItem("contacts")) || [];

displayContacts();

form.addEventListener("submit",(e)=>{

e.preventDefault();

const name =
document.getElementById("name").value.trim();

const email =
document.getElementById("email").value.trim();

const phone =
document.getElementById("phone").value.trim();

const emailPattern =
/^[^\s@]+@[^\s@]+\.[^\s@]+$/;

const phonePattern =
/^[0-9]{10}$/;

if(name.length < 3){
error.textContent =
"Name must contain at least 3 characters";
return;
}

if(!emailPattern.test(email)){
error.textContent =
"Enter valid email address";
return;
}

if(!phonePattern.test(phone)){
error.textContent =
"Phone number must contain 10 digits";
return;
}

error.textContent="";

const contact = {
id: Date.now(),
name,
email,
phone
};

contacts.push(contact);

saveContacts();

displayContacts();

form.reset();

});

function displayContacts(){

contactList.innerHTML="";

contacts.forEach(contact=>{

const card =
document.createElement("div");

card.classList.add("contact-card");

card.innerHTML=`
<h3>${contact.name}</h3>
<p>📧 ${contact.email}</p>
<p>📱 ${contact.phone}</p>

<button
class="delete-btn"
onclick="deleteContact(${contact.id})">
Delete
</button>
`;

contactList.appendChild(card);

});

}

function deleteContact(id){

contacts =
contacts.filter(
contact=>contact.id!==id
);

saveContacts();

displayContacts();

}

function saveContacts(){

localStorage.setItem(
"contacts",
JSON.stringify(contacts)
);

}

search.addEventListener("keyup",()=>{

const value =
search.value.toLowerCase();

const cards =
document.querySelectorAll(".contact-card");

cards.forEach(card=>{

const text =
card.textContent.toLowerCase();

card.style.display =
text.includes(value)
? "block"
: "none";

});

});

</script>

</body>
</html>
