# tollbooth_controller
Key Features : Automatic gate opening after toll is paid, Automatic gate closing once the vehicle crosses.,Prevents multiple vehicles from entering simultaneously.   Applications : Highway toll plazas,Parking lot management,Restricted access control systems,Smart transportation infrastructure.  
# Tollbooth Controller 🚦🛣️  

## 📌 Overview  
This project is a **Tollbooth Gate Controller** designed to automate the **opening and closing of a barrier gate**.  
It ensures smooth vehicle passage after toll collection and prevents unauthorized access.  

👉 *One-line tagline:*  
**“A digital tollbooth gate controller using FSM to automate vehicle entry and exit.”**

---

## 🔹 Features  
- Automatic **gate opening** when a vehicle is detected and toll is paid.  
- Automatic **gate closing** once the vehicle passes.  
- Safety mechanism to **prevent multiple vehicles** from entering at once.  
- Can be implemented on **FPGA / Microcontroller / Digital Circuit**.  
- Extendable for **RFID, IoT, or payment systems**.  


---

## 🔹 Finite State Machine (FSM) Design  
Example states for the controller:  
1. **Idle** – Waiting for a vehicle.  
2. **Toll Verification** – Check if payment is successful.  
3. **Gate Open** – Allow vehicle passage.  
4. **Gate Close** – Close after vehicle crosses.    

---

## 🔹 Implementation  
- **Language/Platform:** Verilog / VHDL / C (depending on your implementation)  
- **Tools Used:** Quartus / Xilinx Vivado / Arduino IDE / Proteus  
- **Hardware Requirements (if any):**  
  - IR Sensor / RFID  
  - Servo Motor / DC Motor with Driver  
  - FPGA Board / Microcontroller  

---

## 🔹 How to Run  
1. Clone this repository:  
   ```bash
   git clone https://github.com/your-username/tollbooth-controller.git
   cd tollbooth-controller

   
---


