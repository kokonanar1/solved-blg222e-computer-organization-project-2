Download Link: https://assignmentchef.com/product/solved-blg222e-computer-organization-project-2
<br>









Design a <strong>hardwired control unit</strong> for the following architecture. Use the structure that you have designed in <strong>Part 4 of Project 1</strong>.




<h1>INSTRUCTION FORMAT</h1>




The instructions are stored in memory in <strong>little endian order</strong>. Since the RAM in Project 1 has 8-bit output, <strong>IR</strong> cannot be filled in one clock cycle. Fortunately, you have <strong>L/H</strong> signal in the <strong>IR</strong> and you can load MSB and LSB separately in two clock cycles:

<ul>

 <li>In the first clock cycle, <strong>LSB</strong> of the instruction must be loaded from an address <strong>A</strong> of the memory to <strong>LSB of IR</strong> [i.e., <strong>IR(7-0)</strong>].</li>

</ul>




<ul>

 <li>In the second clock cycle, <strong>MSB</strong> of the instruction must be loaded from an address <strong>A+1</strong> of the memory to <strong>MSB of IR</strong> [i.e., <strong>IR(15-8)</strong>].</li>

</ul>




There are two types of instructions as described below.




<ul>

 <li>Instructions with address reference have the format shown in Figure 1:</li>

</ul>







<ul>

 <li>The <strong>OPCODE</strong> is a <strong>4-bit</strong> field (See Table 1 for the definition).</li>

</ul>




<ul>

 <li>The next bit [i.e., <strong>IR(11)</strong>] is unused, you set it as 0</li>

</ul>




<ul>

 <li>The <strong>ADDRESSING MODE</strong> is a <strong>1-bit</strong> field (See Table 3 for the definition).</li>

</ul>




<ul>

 <li>The <strong>REGSEL</strong> is a <strong>2-bit</strong> field (See left side of Table 2 for the definition).</li>

</ul>




<ul>

 <li>The <strong>ADDRESS</strong> is an <strong>8-bit</strong></li>

</ul>




<table width="602">

 <tbody>

  <tr>

   <td width="135">OPCODE (4-bit)</td>

   <td width="61">0 (1-bit)</td>

   <td width="191">ADDRESSING MODE (1-bit)</td>

   <td width="102">REGSEL (2-bit)</td>

   <td width="114">ADDRESS (8-bit)</td>

  </tr>

 </tbody>

</table>

<em>Figure 1: Instructions with an address reference </em>




<ul>

 <li>Instructions without address reference have the format shown in Figure 2:</li>

</ul>




<ul>

 <li>The <strong>OPCODE</strong> is a <strong>4-bit</strong> field (See Table 1 for the definition).</li>

</ul>




<ul>

 <li>The <strong>DESTREG</strong> is a <strong>4-bit</strong> field which specifies the destination register (See right side of Table 2 for the definition).</li>

</ul>




<ul>

 <li>The <strong>SRCREG1</strong> is a <strong>4-bit</strong> field which specifies the first source register (See right side of Table 2 for the definition).</li>

</ul>




<ul>

 <li>The <strong>SRCREG2</strong> is a <strong>4-bit</strong> field which specifies the second source register (See right side of Table 2 for the definition).</li>

</ul>




<table width="602">

 <tbody>

  <tr>

   <td width="133">OPCODE (4-bit)</td>

   <td width="147">DESTREG (4-bit)</td>

   <td width="172">SRCREG1 (4-bit)</td>

   <td width="151">SRCREG2 (4-bit)</td>

  </tr>

 </tbody>

</table>

<em>Figure 2: Instructions without an address reference</em>

<h2>Table 1: OPCODE field and SYMBols for operations and their descriptions</h2>




<em>Table 2: REGSEL (Left) and DESTREG/SRCREG1/SRCREG2 (Right) select the register of </em>

<h2>interest for a particular instruction</h2>




<h2>Table 3: Addressing modes</h2>




<h1>EXAMPLE</h1>




Since PC value is initially 0, your code first executes the instruction in memory address 0x00. This instruction is <strong>BRA START_ADDRESS</strong> where <strong>START_ADDRESS</strong> is the starting address of your instructions.




The code given below adds data that are stored at memory addresses 0xA0, 0xA1, 0xA2, 0xA3, and 0xA4 (i.e., calculates <strong>total</strong> = M[A0] + M[A1] + M[A2] + M[A3] + M[A4]). Then, it stores the <strong>total</strong> in memory address 0xA6 (i.e., M[A6]). It is written as a loop that iterates 5 times.




You have to determine the binary code of the program and write it to the memory. Your final logisim implementation is expected to fetch the instructions starting from address 0x00, decode them and execute all instructions, one by one.




BRA  0x20                           # This instruction is written to the memory address 0x00,

# The first instruction must be written to the address 0x20







LD R1 IM 0x05                   # This first instruction is written to the address 0x20,

# R1 is used for iteration number

LD R2 IM 0x00                 # R2 is used to store total

LD R3 IM 0xA0

MOV AR R3                      # AR is used to track data address: starts from 0xA0

<table width="579">

 <tbody>

  <tr>

   <td width="189">LABEL: LD R3 D</td>

   <td width="41"> </td>

   <td width="349"># R3 ← M[AR]            (AR = 0xA0 to 0xA4)</td>

  </tr>

  <tr>

   <td width="189">               ADD R2 R2 R3</td>

   <td width="41"> </td>

   <td width="349"># R2 ← R2 + R3          (Total = Total + M[AR])</td>

  </tr>

  <tr>

   <td width="189">            INC AR AR</td>

   <td width="41"> </td>

   <td width="349"># AR ← AR + 1           (Next Data)</td>

  </tr>

  <tr>

   <td width="189">               DEC R1 R1</td>

   <td width="41"> </td>

   <td width="349"># R1 ← R1 – 1               (Decrement Iteration Counter)</td>

  </tr>

  <tr>

   <td width="189">               BNE IM LABEL</td>

   <td width="41"> </td>

   <td width="349"># Go back to LABEL if Z=0 (Iteration Counter &gt; 0)</td>

  </tr>

  <tr>

   <td width="189">               INC AR AR</td>

   <td width="41"> </td>

   <td width="349"># AR ← AR + 1            (Total will be written to 0xA6)</td>

  </tr>

  <tr>

   <td width="189">               ST  R2 D</td>

   <td width="41"> </td>

   <td width="349"># M[AR] ← R2            (Store Total at 0xA6)</td>

  </tr>

 </tbody>

</table>







<strong> </strong>

<strong> </strong>


