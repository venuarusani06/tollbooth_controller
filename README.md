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
   implementation:
   module tollbooth_controller (
    input clk,
    input reset,
    input vehicle_detected,
    input payment_done,
    output reg barrier_open,
    output reg barrier_close,
    output reg [1:0] led_status  // 00=Idle, 01=Wait, 10=Open, 11=Close
);

    // State encoding
    parameter IDLE = 2'b00,
              WAIT_PAYMENT = 2'b01,
              OPEN_GATE = 2'b10,
              CLOSE_GATE = 2'b11;

    reg [1:0] state, next_state;

    // State register
    always @(posedge clk or posedge reset) begin
        if (reset)
            state <= IDLE;
        else
            state <= next_state;
    end

    // Next state logic
    always @(*) begin
        case(state)
            IDLE: 
                if (vehicle_detected)
                    next_state = WAIT_PAYMENT;
                else
                    next_state = IDLE;

            WAIT_PAYMENT: 
                if (payment_done)
                    next_state = OPEN_GATE;
                else
                    next_state = WAIT_PAYMENT;

            OPEN_GATE: 
                next_state = CLOSE_GATE;

            CLOSE_GATE: 
                if (!vehicle_detected)
                    next_state = IDLE;
                else
                    next_state = CLOSE_GATE;

            default: next_state = IDLE;
        endcase
    end

    // Output logic
    always @(*) begin
        barrier_open = 0;
        barrier_close = 0;
        led_status = 2'b00;

        case(state)
            IDLE: begin
                barrier_close = 1;
                led_status = 2'b00;
            end
            WAIT_PAYMENT: begin
                barrier_close = 1;
                led_status = 2'b01;
            end
            OPEN_GATE: begin
                barrier_open = 1;
                led_status = 2'b10;
            end
            CLOSE_GATE: begin
                barrier_close = 1;
                led_status = 2'b11;
            end
        endcase
    end

endmodule

test bench:
`timescale 1ns/1ps
module tollbooth_tb();

    reg clk, reset, vehicle_detected, payment_done;
    wire barrier_open, barrier_close;
    wire [1:0] led_status;

    // Instantiate DUT
    tollbooth_controller uut (
        .clk(clk),
        .reset(reset),
        .vehicle_detected(vehicle_detected),
        .payment_done(payment_done),
        .barrier_open(barrier_open),
        .barrier_close(barrier_close),
        .led_status(led_status)
    );

    // Clock generation (10ns period)
    always #5 clk = ~clk;

    initial begin
        // Enable waveform dump
        $dumpfile("dump.vcd");     // VCD file name
        $dumpvars(0, tollbooth_tb); // Dump all signals in testbench

        // Initialize
        clk = 0; reset = 1;
        vehicle_detected = 0; payment_done = 0;
        #10 reset = 0;

        // Scenario 1: Vehicle arrives, pays, passes
        vehicle_detected = 1; payment_done = 0;
        #20;
        payment_done = 1; #20;
        payment_done = 0; #20;
        vehicle_detected = 0; #20;

        // Scenario 2: Another vehicle, delayed payment
        vehicle_detected = 1; #30;
        payment_done = 1; #20;
        payment_done = 0; #20;
        vehicle_detected = 0; #20;

        // Scenario 3: Vehicle comes but does not pay
        vehicle_detected = 1; payment_done = 0;
        #50;
        vehicle_detected = 0; #20;

        #100 $finish; // finish simulation
    end
endmodule
   
---


