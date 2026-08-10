# 4-Bit ALU using Verilog

## 📌 Project Overview

This project implements a **4-bit Arithmetic Logic Unit (ALU)** using Verilog HDL.

An ALU is a fundamental digital circuit used in processors and digital systems to perform arithmetic and logical operations.

This ALU accepts two 4-bit inputs and a 3-bit operation selection signal.

---

## 🎯 Objective

The objective of this project is to design and verify a 4-bit ALU capable of performing arithmetic and logical operations.

The project demonstrates:

* Combinational logic
* Arithmetic operations
* Logic operations
* Status flags
* Verilog case statements
* Testbench-based verification

---

## 🛠️ Technologies Used

* Verilog HDL
* VS Code
* Icarus Verilog
* ModelSim / QuestaSim
* GTKWave (optional)
* GitHub

---

## 📂 Project Structure

```text
4-Bit-ALU/
│
├── README.md
├── src/
│   └── alu_4bit.v
│
├── testbench/
│   └── tb_alu_4bit.v
│
└── simulation/
    └── simulation_results.txt
```

---

## 🔢 Inputs and Outputs

| Signal | Width | Description        |
| ------ | ----: | ------------------ |
| A      | 4-bit | First input        |
| B      | 4-bit | Second input       |
| OP     | 3-bit | Operation selector |
| RESULT | 4-bit | Operation result   |
| CARRY  | 1-bit | Carry flag         |
| BORROW | 1-bit | Borrow flag        |
| ZERO   | 1-bit | Zero flag          |

---

## ⚙️ Operation Table

| OP    | Operation | Description        |
| ----- | --------- | ------------------ |
| `000` | A + B     | Addition           |
| `001` | A - B     | Subtraction        |
| `010` | A AND B   | Bitwise AND        |
| `011` | A OR B    | Bitwise OR         |
| `100` | A XOR B   | Bitwise XOR        |
| `101` | NOT A     | Bitwise complement |
| `110` | A + 1     | Increment          |
| `111` | A - 1     | Decrement          |

---

## 🚩 Status Flags

### Carry Flag

The Carry flag is set when an arithmetic operation produces a carry beyond the 4-bit result.

Example:

```text
1111 + 0001 = 1 0000
```

Therefore:

```text
RESULT = 0000
CARRY  = 1
```

---

### Borrow Flag

The Borrow flag is set when the subtraction requires borrowing.

Example:

```text
0011 - 0101
```

Since `3 < 5`:

```text
BORROW = 1
```

---

### Zero Flag

The Zero flag is set when the result is:

```text
0000
```

For example:

```text
1111 + 0001 = 0000
```

with a carry.

Therefore:

```text
ZERO = 1
CARRY = 1
```

---

## 🧪 Testbench

The testbench verifies:

* Addition
* Addition with carry
* Subtraction
* Subtraction with borrow
* AND
* OR
* XOR
* NOT
* Increment
* Decrement
* Zero-result detection

---

## ▶️ Simulation Using Icarus Verilog

Open the terminal inside the project directory.

### Compile

```bash
iverilog -o alu_sim src/alu_4bit.v testbench/tb_alu_4bit.v
```

### Run

```bash
vvp alu_sim
```

---

## 📋 Expected Output

```text
==============================================================
                    4-BIT ALU TEST
==============================================================
A=0011 B=0010 OP=000 | RESULT=0101 CARRY=0 BORROW=0 ZERO=0
A=1001 B=0011 OP=001 | RESULT=0110 CARRY=0 BORROW=0 ZERO=0
A=1100 B=1010 OP=010 | RESULT=1000 CARRY=0 BORROW=0 ZERO=0
A=1100 B=1010 OP=011 | RESULT=1110 CARRY=0 BORROW=0 ZERO=0
A=1100 B=1010 OP=100 | RESULT=0110 CARRY=0 BORROW=0 ZERO=0
A=1010 B=0000 OP=101 | RESULT=0101 CARRY=0 BORROW=0 ZERO=0
A=1111 B=0000 OP=110 | RESULT=0000 CARRY=1 BORROW=0 ZERO=1
A=0101 B=0000 OP=111 | RESULT=0100 CARRY=0 BORROW=0 ZERO=0
A=1111 B=0001 OP=000 | RESULT=0000 CARRY=1 BORROW=0 ZERO=1
A=0011 B=0101 OP=001 | RESULT=1110 CARRY=0 BORROW=1 ZERO=0
A=1010 B=0101 OP=010 | RESULT=0000 CARRY=0 BORROW=0 ZERO=1
A=0000 B=0000 OP=110 | RESULT=0001 CARRY=0 BORROW=0 ZERO=0
A=0000 B=0000 OP=111 | RESULT=1111 CARRY=0 BORROW=1 ZERO=0
==============================================================
```

---

## 📚 Concepts Demonstrated

* Arithmetic Logic Unit
* Binary addition
* Binary subtraction
* Incrementer
* Decrementer
* AND operation
* OR operation
* XOR operation
* NOT operation
* Status flags
* Combinational logic
* Verilog `case` statement
* Testbench development
* Functional verification

---

## 🚀 Future Improvements

This ALU can be extended to:

* 8-bit ALU
* 16-bit ALU
* 32-bit ALU
* Multiplication operation
* Division operation
* Shift operations
* Rotate operations
* Carry flag
* Overflow flag
* Negative/sign flag
* FPGA implementation
* Processor datapath

---

## 👩‍💻 Author

**Harshu**

B.Tech - Electronics and Communication Engineering

---

## 📄 License

This project is created for educational and academic purposes.
```verilog
`timescale 1ns/1ps

module alu_4bit (
    input  wire [3:0] a,
    input  wire [3:0] b,
    input  wire [2:0] op,
    output reg  [3:0] result,
    output reg        carry,
    output reg        borrow,
    output wire       zero
);

    reg [4:0] temp;

    always @(*) begin

        // Default values
        result = 4'b0000;
        carry  = 1'b0;
        borrow = 1'b0;
        temp   = 5'b00000;

        case (op)

            // 000: Addition
            3'b000: begin
                temp   = {1'b0, a} + {1'b0, b};
                result = temp[3:0];
                carry  = temp[4];
            end

            // 001: Subtraction
            3'b001: begin
                result = a - b;
                borrow = (a < b);
            end

            // 010: AND
            3'b010: begin
                result = a & b;
            end

            // 011: OR
            3'b011: begin
                result = a | b;
            end

            // 100: XOR
            3'b100: begin
                result = a ^ b;
            end

            // 101: NOT A
            3'b101: begin
                result = ~a;
            end

            // 110: Increment A
            3'b110: begin
                temp   = {1'b0, a} + 5'b00001;
                result = temp[3:0];
                carry  = temp[4];
            end

            // 111: Decrement A
            3'b111: begin
                result = a - 4'b0001;
                borrow = (a == 4'b0000);
            end

            default: begin
                result = 4'b0000;
            end

        endcase

    end

    // ZERO flag
    assign zero = (result == 4'b0000);

endmodule
```
```verilog
`timescale 1ns/1ps

module tb_alu_4bit;

    reg [3:0] a;
    reg [3:0] b;
    reg [2:0] op;

    wire [3:0] result;
    wire carry;
    wire borrow;
    wire zero;

    alu_4bit DUT (
        .a(a),
        .b(b),
        .op(op),
        .result(result),
        .carry(carry),
        .borrow(borrow),
        .zero(zero)
    );

    task test_operation;
        input [3:0] test_a;
        input [3:0] test_b;
        input [2:0] test_op;

        begin
            a  = test_a;
            b  = test_b;
            op = test_op;

            #10;

            $display(
                "A=%b B=%b OP=%b | RESULT=%b CARRY=%b BORROW=%b ZERO=%b",
                a, b, op, result, carry, borrow, zero
            );
        end
    endtask

    initial begin

        $display("==============================================================");
        $display("                    4-BIT ALU TEST");
        $display("==============================================================");

        // 000: A + B
        test_operation(4'b0011, 4'b0010, 3'b000);

        // 001: A - B
        test_operation(4'b1001, 4'b0011, 3'b001);

        // 010: A AND B
        test_operation(4'b1100, 4'b1010, 3'b010);

        // 011: A OR B
        test_operation(4'b1100, 4'b1010, 3'b011);

        // 100: A XOR B
        test_operation(4'b1100, 4'b1010, 3'b100);

        // 101: NOT A
        test_operation(4'b1010, 4'b0000, 3'b101);

        // 110: A + 1
        test_operation(4'b1111, 4'b0000, 3'b110);

        // 111: A - 1
        test_operation(4'b0101, 4'b0000, 3'b111);

        // Additional tests

        // Addition with carry
        test_operation(4'b1111, 4'b0001, 3'b000);

        // Subtraction with borrow
        test_operation(4'b0011, 4'b0101, 3'b001);

        // AND producing zero
        test_operation(4'b1010, 4'b0101, 3'b010);

        // Increment zero
        test_operation(4'b0000, 4'b0000, 3'b110);

        // Decrement zero
        test_operation(4'b0000, 4'b0000, 3'b111);

        $display("==============================================================");

        $finish;

    end

endmodule
```
# 4-BIT ALU SIMULATION RESULTS

A=0011 B=0010 OP=000 | RESULT=0101 CARRY=0 BORROW=0 ZERO=0
A=1001 B=0011 OP=001 | RESULT=0110 CARRY=0 BORROW=0 ZERO=0
A=1100 B=1010 OP=010 | RESULT=1000 CARRY=0 BORROW=0 ZERO=0
A=1100 B=1010 OP=011 | RESULT=1110 CARRY=0 BORROW=0 ZERO=0
A=1100 B=1010 OP=100 | RESULT=0110 CARRY=0 BORROW=0 ZERO=0
A=1010 B=0000 OP=101 | RESULT=0101 CARRY=0 BORROW=0 ZERO=0
A=1111 B=0000 OP=110 | RESULT=0000 CARRY=1 BORROW=0 ZERO=1
A=0101 B=0000 OP=111 | RESULT=0100 CARRY=0 BORROW=0 ZERO=0

A=1111 B=0001 OP=000 | RESULT=0000 CARRY=1 BORROW=0 ZERO=1
A=0011 B=0101 OP=001 | RESULT=1110 CARRY=0 BORROW=1 ZERO=0
A=1010 B=0101 OP=010 | RESULT=0000 CARRY=0 BORROW=0 ZERO=1
A=0000 B=0000 OP=110 | RESULT=0001 CARRY=0 BORROW=0 ZERO=0
A=0000 B=0000 OP=111 | RESULT=1111 CARRY=0 BORROW=1 ZERO=0

==============================================================
Simulation completed successfully.
==================================
