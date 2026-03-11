"use client";
import { useState, useEffect } from "react";

export default function Home() {

const [network,setNetwork]=useState("");
const [bundle,setBundle]=useState("");
const [phone,setPhone]=useState("");
const [orders,setOrders]=useState([]);
const [admin,setAdmin]=useState(false);
const [password,setPassword]=useState("");

const ADMIN_PASSWORD="1234";
const MOMO_NUMBER="054XXXXXXX";
const WHATSAPP="23354XXXXXXX";

const bundles = [
{size:"1GB",price:5},
{size:"2GB",price:9},
{size:"5GB",price:20},
{size:"10GB",price:35},
];

useEffect(()=>{
const data=localStorage.getItem("orders");
if(data){setOrders(JSON.parse(data))}
},[])

useEffect(()=>{
localStorage.setItem("orders",JSON.stringify(orders))
},[orders])

function submitOrder(){

if(!network||!bundle||!phone){
alert("Fill all fields");
return;
}

const order={
id:Date.now(),
network,
bundle,
phone,
status:"Pending",
date:new Date().toLocaleString()
}

setOrders([order,...orders])

const message=`New Data Order
Network: ${network}
Bundle: ${bundle}
Phone: ${phone}`

window.open(`https://wa.me/${WHATSAPP}?text=${encodeURIComponent(message)}`)

setNetwork("")
setBundle("")
setPhone("")

alert("Order sent successfully. Please complete payment.")

}

function markDelivered(id){

const updated=orders.map(o=>{
if(o.id===id){
return {...o,status:"Delivered"}
}
return o
})

setOrders(updated)

}

if(admin && password!==ADMIN_PASSWORD){
return(

<div style={{padding:40,textAlign:"center"}}>

<h2>Admin Login</h2>

<input
type="password"
placeholder="Enter password"
onChange={(e)=>setPassword(e.target.value)}
style={{padding:10,width:200}}
/>

</div>

)
}

return(

<div style={{fontFamily:"Arial",padding:20,maxWidth:600,margin:"auto"}}>

<h1 style={{textAlign:"center"}}>⚡ Ghana Data Hub</h1>
<p style={{textAlign:"center"}}>Cheap & Fast Data Bundles</p>

{!admin && (

<div style={{border:"1px solid #ddd",padding:20,borderRadius:10}}>

<h3>Buy Data</h3>

<select
value={network}
onChange={(e)=>setNetwork(e.target.value)}
style={{padding:10,width:"100%",marginBottom:10}}
>

<option value="">Select Network</option>
<option>MTN</option>
<option>Telecel</option>
<option>AirtelTigo</option>

</select>

<select
value={bundle}
onChange={(e)=>setBundle(e.target.value)}
style={{padding:10,width:"100%",marginBottom:10}}
>

<option value="">Select Data Bundle</option>

{bundles.map((b,i)=>(
<option key={i}>
{b.size} - GH₵{b.price}
</option>
))}

</select>

<input
placeholder="Customer Phone Number"
value={phone}
onChange={(e)=>setPhone(e.target.value)}
style={{padding:10,width:"100%",marginBottom:10}}
/>

<div style={{background:"#f3f3f3",padding:10,marginBottom:10}}>

<p><b>Payment Instructions</b></p>
<p>Send MoMo to: {0507141580/247659110}</p>
<p>After payment click Submit Order</p>
<p>Delivery time: 5-15 minutes</p>

</div>

<button
onClick={submitOrder}
style={{padding:12,width:"100%",background:"black",color:"white"}}
>

Submit Order

</button>

<button
onClick={()=>window.open(`https://wa.me/${WHATSAPP}`)}
style={{padding:10,width:"100%",marginTop:10}}
>

Customer Support (WhatsApp)

</button>

<button
onClick={()=>setAdmin(true)}
style={{padding:10,width:"100%",marginTop:10}}
>

Admin Login

</button>

</div>

)}

{admin && (

<div>

<h3>Admin Dashboard</h3>

{orders.map(o=>(
<div key={o.id} style={{border:"1px solid #ddd",padding:10,marginBottom:10}}>

<p>{o.network} - {o.bundle}</p>
<p>{o.phone}</p>
<p>{o.date}</p>
<p>Status: {o.status}</p>

{o.status==="Pending" && (

<button onClick={()=>markDelivered(o.id)}>
Mark Delivered
</button>

)}

</div>
))}

<button
onClick={()=>setAdmin(false)}
style={{padding:10,width:"100%"}}
>

Back

</button>

</div>

)}

</div>

)

}
