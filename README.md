# VSD Hardware Development Program (May 30 - August 8, 2024)

This repository contains setup instructions for the VSD Hardware Development Program.

<details>
  <summary>Week 0</summary>

  ## Yosys Installation

  1. Update package list and install dependencies:
      ```sh
      sudo apt-get update
      sudo apt-get install build-essential clang bison flex libreadline-dev gawk tcl-dev libffi-dev git graphviz xdot pkg-config python3 libboost-system-dev libboost-python-dev libboost-filesystem-dev zlib1g-dev make
      ```

  2. Clone the Yosys repository and install:
      ```sh
      git clone https://github.com/YosysHQ/yosys.git
      cd yosys
      make config-gcc
      make
      sudo make install
      ```

  ![Yosys Installation Screenshot](https://github.com/siddharthanand3/vsdhdp/assets/171400217/38640060-1f57-4b90-85ce-02d3d8da50b6)

  ## iVerilog Installation

  1. Update package list and install iVerilog:
      ```sh
      sudo apt-get update
      sudo apt-get install iverilog
      ```

  ![iVerilog Installation Screenshot](https://github.com/siddharthanand3/vsdhdp/assets/171400217/0a4109eb-273c-4712-936b-3f2052e3cfb1)

  ## GTK Wave Installation

  1. Update package list and install GTK Wave:
      ```sh
      sudo apt-get update
      sudo apt install gtkwave
      ```

  ![GTK Wave Installation Screenshot](https://github.com/siddharthanand3/vsdhdp/assets/171400217/fe3a3ab1-4a81-4a36-a04c-decf631f9ade)

</details>

<details>
  <summary>Week 1</summary>

### Day 1

#### Viewing the Output After Simulation in GTKWave

1. **Open iVerilog:**
    - ![ss for opening iverilog](https://github.com/siddharthanand3/vsdhdp/assets/171400217/bbd88023-3ee5-4547-af92-191251f8c92a)

2. **Create a VCD file:**
    - Steps:
    ```sh
    iverilog (name of the verilog file).v (tb_(name of verilog file)).v
    ./a.out
    ```
    - ![ss for opening gtkwave after loading the files into iverilog](https://github.com/siddharthanand3/vsdhdp/assets/171400217/9c023f1a-c651-4cfa-bcf2-e514f69397a1)

3. **Open the file in GTKWave to observe output:**
    - Steps:
    ```sh
    gtkwave (tb_(name of the verilog file)).vcd
    ```
    - OUTPUT:
    - ![Screenshot 2024-06-11 003114](https://github.com/siddharthanand3/vsdhdp/assets/171400217/6bbf9384-86a3-4b88-8fc3-092955a237b0)

#### Viewing the Verilog Code for Both the Testbench and the File

- ![iverilog testbench and file](https://github.com/siddharthanand3/vsdhdp/assets/171400217/717bfe71-c7a4-4564-86d4-fcebb9355613)

#### Read Liberty Command to Read Both the .lib File and Verilog Code File

- ![read lib 1](https://github.com/siddharthanand3/vsdhdp/assets/171400217/56b46a7a-631e-4660-9619-c3cd602aed9f)
- ![readverilog](https://github.com/siddharthanand3/vsdhdp/assets/171400217/d049566f-a9c8-4467-bf9e-f8bda294e001)

#### Synthesis Design

- Yosys is the synthesizer used to convert the RTL Design into a netlist for viewing purposes.
- Code:
    ```sh
    read_liberty -lib (.lib file location)
    read_verilog (name of the verilog file).v
    synth -top (module name in the verilog file)
    abc -liberty (.lib file location)
    ```
- ![synthesisdesign](https://github.com/siddharthanand3/vsdhdp/assets/171400217/4d2b9b0b-49b3-4724-b2da-8b40f6db723c)

#### Realize the Exact .lib File and Obtain Parameters for Verification

- ![realisesky130_cd](https://github.com/siddharthanand3/vsdhdp/assets/171400217/3b53b11f-aed1-4861-9bda-9d96e9c0c53c)

#### Netlist Viewing

- Code:
    ```sh
    show
    ```
- ![netlist](https://github.com/siddharthanand3/vsdhdp/assets/171400217/b55cb99e-59a1-4503-ab0e-295d2aa938a9)

#### Writing the Verilog Netlist File

- Code:
    ```sh
    write_verilog good_mux_netlist.v
    !gvim good_mux_netlist.v
    ```
- ![netlist representation](https://github.com/siddharthanand3/vsdhdp/assets/171400217/d73a9989-b0a8-40e5-8da8-13f942f19803)

### Day 2

#### Accessing the .lib File

- ![lib ss](https://github.com/siddharthanand3/vsdhdp/assets/171400217/70ad16b6-d6ef-4d75-a96d-5c1ec3e603b3)

#### An Example for How Cells Are Stored

- ![and gate specifications](https://github.com/siddharthanand3/vsdhdp/assets/171400217/c3c1af4f-98fe-413f-afb6-94e4c11484dc)
- Different AND gates have different sizes and power consumed. For example, in the below figure although the AND4 gate occupies more area, the delay is lesser as compared to AND2 and AND0.
- ![andgatesdifferent flavors](https://github.com/siddharthanand3/vsdhdp/assets/171400217/ad8e0ced-1908-4de6-9611-ede56eaa930f)

#### Synthesis of Multiple Modules

- When a single module is used multiple times in a file, it is created only once and replicated to fit the requirement. This saves time and power.
- Code:
    ```sh
    read_liberty -lib (path to the .lib file)
    read_verilog (name of the Verilog file)
    synth_top (name of the module)
    abc -liberty (path to the .lib file)
    show
    ```
- ![multi code](https://github.com/siddharthanand3/vsdhdp/assets/171400217/18d34fc2-97a0-4bf9-86ac-a7eb8815d4c8)
- ![Multiple modules](https://github.com/siddharthanand3/vsdhdp/assets/171400217/bbdb9291-cf46-4a79-9b2e-fc8c14e0af55)

#### Design Output for Each Submodule

- ![modules design output](https://github.com/siddharthanand3/vsdhdp/assets/171400217/f3e49ebb-87d2-4428-b1f4-94b56407fc3c)

#### Hierarchical Design

- The design is constituted of many submodules, and it is preserved.
- Code:
    ```sh
    read_liberty -lib <path to the .lib file>
    read_verilog (name of the Verilog file)
    synth_top (name)
    abc -liberty (path to the .lib file)
    show (name given)
    ```
- ![hierarchical design](https://github.com/siddharthanand3/vsdhdp/assets/171400217/418c90a1-3456-4ae4-ab94-c1ea064635ed)
- ![hierarchy is preserved](https://github.com/siddharthanand3/vsdhdp/assets/171400217/1ab9c081-60cc-4645-ad25-dc90a65bcc7c)
- Code:
    ```sh
    write_verilog -noattr (name)
    !gvim (name)
    ```

#### Flattened File

- On using the 'flatten' command in Yosys you can break down the submodules.
- Code:
    ```sh
    flatten
    write_verilog (name of the module)_flat
    !gvim (name of the module)_flat
    ```
- ![flatten comparision](https://github.com/siddharthanand3/vsdhdp/assets/171400217/1e0cf6cb-1774-4803-822e-6bebfa6ac6f9)
- ![flattened netlist](https://github.com/siddharthanand3/vsdhdp/assets/171400217/91ed3a28-ec7f-4065-8f38-928a740bc226)

#### Synthesizing the Submodules Separately

- Doing so helps efficiency and reduces delay.
- ![synthesising on submodule1](https://github.com/siddharthanand3/vsdhdp/assets/171400217/9e6e37fc-a678-4b4e-af03-b206d3ac4d4b)

#### Netlist of Submodule1

- ![netlist submodule1](https://github.com/siddharthanand3/vsdhdp/assets/171400217/4f60bb8c-f38f-4ec9-ae56-eb46b78f4791)

#### Flop Synthesis Simulations

- Code:
    - To view in GTKWave:
        ```sh
        iverilog (name of the verilog file).v tb_(name of the verilog file).v
        ./a.out
        gtkwave tb_(name of the verilog file).v
        ```
    - For viewing netlist in Yosys:
        ```sh
        yosys
        read_liberty -lib (path to the .lib file)
        read_verilog (name of the verilog file).v
        dfflibmap -liberty (path to the .lib file)
        abc -liberty (path to the .lib file)
        show
        ```

#### Asynchronous Reset

- ![asyncres](https://github.com/siddharthanand3/vsdhdp/assets/171400217/87a3f977-9398-4950-a24b-ef3cf3877201)
- ![dff asyncreset netlist](https://github.com/siddharthanand3/vsdhdp/assets/171400217/a200e916-f8ce-4675-b058-7fb515ab7934)

#### Asynchronous Set

- ![async set](https://github.com/siddharthanand3/vsdhdp/assets/171400217/b1caee83-d554-4f2c-b9e9-eb34c6693631)
- ![asynset flop netlist](https://github.com/siddharthanand3/vsdhdp/assets/171400217/2903222e-2593-448f-84c5-ae5268412577)

#### Synchronous Set

- ![syncres](https://github.com/siddharthanand3/vsdhdp/assets/171400217/41b5ed2f-f264-4e66-9ad8-81f489941bf2)
- ![syncres netlist](https://github.com/siddharthanand3/vsdhdp/assets/171400217/304ea5a9-6cc9-4ef2-b263-fb7dc11a191f)

</details>


Week 2

Day 3

Logic Optimization:

Logic optimization is a process of finding an equivalent representation of the specified logic circuit under one or more specified constraints. This process is a part of a logic synthesis applied in digital electronics and integrated circuit design.


Combinational logic optimizaation:

Steps:

1) In the verilog files folder, open yosys.
2) read_liberty -lib (path to the .lib file)
3) read_verilog opt_check.v
4) synth -top opt_check
5) opt_clean -purge
6) abc -liberty (path to the .lib file)
7) show


Opt_check file:

![Screenshot 2024-06-17 153750](https://github.com/siddharthanand3/vsdhdp/assets/171400217/ed137704-a63e-427a-ab6d-01b974ac73f9)

![Screenshot 2024-06-17 134451](https://github.com/siddharthanand3/vsdhdp/assets/171400217/157083a9-8d98-4263-b849-bb45faca0a36)


Opt_check2 file:

![optcheck2file](https://github.com/siddharthanand3/vsdhdp/assets/171400217/ea231afd-bd5e-4fa4-ad05-bf6e7aac3892)

![optcheck2](https://github.com/siddharthanand3/vsdhdp/assets/171400217/32b06234-f2a1-4261-b4a3-386bc211c161)


Opt_check3 file:

![optcheck3file](https://github.com/siddharthanand3/vsdhdp/assets/171400217/e82c3999-5ce4-4c5c-a248-1896cf69f660)

![opt_check3 netlist](https://github.com/siddharthanand3/vsdhdp/assets/171400217/60dc3739-13e7-4a87-9115-b58b6bdf2a65)


Opt_check4 file:

![opt_check4file](https://github.com/siddharthanand3/vsdhdp/assets/171400217/b73f7e05-67bc-4a09-aafa-15fe2b0083c8)

![opt_check4 netlist](https://github.com/siddharthanand3/vsdhdp/assets/171400217/f4da383e-fc5f-44cb-b11c-4ed2ac2a95b6)


Optimization of multiple modules:

In order to optimize multiple modules at once, we need to flatten it first.

Code:

1) yosys
2) read_liberty -lib (path to .lib file)
3) read_verilog (name of the file).v
4) synth -top (name of the module)
5) flatten
6) write_verilog (name of the file)_flat.v
7) opt_clean -purge
8) abc -liberty (path to the .lib file)
9) show


Multiple_module_opt.v:

![multiplemoduleopt file](https://github.com/siddharthanand3/vsdhdp/assets/171400217/1bc28c90-2bd4-4997-ba8b-002571f07fbd)

![multiplemoduleopt](https://github.com/siddharthanand3/vsdhdp/assets/171400217/5ac68ad1-f45f-4abd-b7d3-b55247224f37)




Sequential logic optimization:

Steps:

GTKWave:

1) Open the verilog files folder.
2) iverilog (name of the verilog file).v tb_(name of the verilog file).v
3) ./a.out
4) gtkwave tb_(name of the verilog file).v

Yosys netlist:

1) yosys
2) read_liberty -lib (path to the .lib file)
3) read_verilog (name of the verilog file).v

Since we are using a D flip flop, we use a keyword called 'dfflibmap':

4) dfflibmap -liberty (path to the .lib file)

This allows us to access only the dff files in the library.

5) abc -liberty (path to the .lib file)
6) show


Dff_const1.v:

![dff_const1 file](https://github.com/siddharthanand3/vsdhdp/assets/171400217/d3ba78a9-f06c-4e63-ae34-1f8861d23912)

![tb_const1 gtk](https://github.com/siddharthanand3/vsdhdp/assets/171400217/547008c0-eba6-4650-9858-b6e269184e98)

![dffconst1 netlist](https://github.com/siddharthanand3/vsdhdp/assets/171400217/31547a10-e59a-4112-ae4b-d0680373dd8a)


Dff_const2.v:

![dff_const2 file](https://github.com/siddharthanand3/vsdhdp/assets/171400217/549e0cae-bdc7-4788-befe-0c28ae431e0d)

![tb_const2 gtk](https://github.com/siddharthanand3/vsdhdp/assets/171400217/47e1d220-fa27-4e11-b911-4969644b2f6a)

![dff_const2 netlist](https://github.com/siddharthanand3/vsdhdp/assets/171400217/19621b92-7790-4195-bbb9-e98b4f58e7aa)


Dff_const3.v:

![dff_const3 file](https://github.com/siddharthanand3/vsdhdp/assets/171400217/37c15e31-f4bf-42f8-8878-bbce8382933e)

![gtkwave const3](https://github.com/siddharthanand3/vsdhdp/assets/171400217/575ac287-7b79-42ae-82cf-9f29eccb8a71)

![const3 netlist](https://github.com/siddharthanand3/vsdhdp/assets/171400217/7593754d-f778-4ebc-8b8f-cf1433e12449)


Dff_const4.v:

![dff_const4 file](https://github.com/siddharthanand3/vsdhdp/assets/171400217/7f49bdec-cd01-4fe6-8da3-52bcb511ef6b)

![gtkwave const4](https://github.com/siddharthanand3/vsdhdp/assets/171400217/029d50b0-1a2f-4a64-ac15-29d43f53cd32)

![const4 netlist](https://github.com/siddharthanand3/vsdhdp/assets/171400217/51796e6a-0c52-4ec8-b664-b63fdd8c5230)


Dff_const5.v:

![dff_const5 file](https://github.com/siddharthanand3/vsdhdp/assets/171400217/97c85158-cb06-4fad-be5b-89260485def1)

![gtkwave const5](https://github.com/siddharthanand3/vsdhdp/assets/171400217/3955d1cc-3d1f-4ec1-a085-8274cf9aa10f)

![dff_const5 netlist](https://github.com/siddharthanand3/vsdhdp/assets/171400217/fa6d846c-8a42-4a08-b011-e2ad86870dfe)



Day 4


Gate level simulation (GLS)


Synthesis simulation mismatch:


Steps:

1) GTKWave simulation
2) Yosys synthesis of netlist
3) Gate level simulation to compare the two simulations and confirm

Code:

GTKWave simulation:

1) iverilog (name of the verilog file).v tb_(name of the verilog file).v
2) ./a.out
3) gtkwave tb_(name of the verilog file).v


Yosys synthesis of netlist:

 read_liberty -lib (path to the .lib file)
 read_verilog (name of the verilog file).v
 synth -top (name of the module)
 abc -liberty (path to the .lib file)
 write_verilog (name of the verilog file)_net.v
 show


Gate level simulation:

1) iverilog (path to the primitives.v file) (path to the sky130_fd_sc_hd.v file) (name of the verilog file)_net.v (testbench of the verilog file)
2) ./a.out
3) gtkwave (testbench of the verilog file).vcd



Ternary_mux_operator.v:


File:

![ternarymuxfile](https://github.com/siddharthanand3/vsdhdp/assets/171400217/e3a704ac-9b72-47c8-89b0-be29492823c5)


GTKWave simulation:

![gtkwave ternary mux](https://github.com/siddharthanand3/vsdhdp/assets/171400217/22b26915-2e98-4f00-8707-5bb375750505)


Netlist:

![netlistternarymux](https://github.com/siddharthanand3/vsdhdp/assets/171400217/882805fb-8763-49a5-91fe-dba2548dd597)


Confirmed GLS output:

![gls output ternary mux](https://github.com/siddharthanand3/vsdhdp/assets/171400217/7e832ae8-a720-4181-80dc-3aa6d88677f3)


Since the GLS output is matching the initial GTKWave simulation the gate level simulation is complete.



Bad_mux.v:


File:

![badmuxfile](https://github.com/siddharthanand3/vsdhdp/assets/171400217/11de45ac-99d6-4798-9e68-0e2cd1d7ad3a)


GTKWave simulation:

![badmux gtkwave](https://github.com/siddharthanand3/vsdhdp/assets/171400217/4a4116cf-b540-4800-90b6-4798d8b56096)


Netlist:

![badmux netlist](https://github.com/siddharthanand3/vsdhdp/assets/171400217/f59a16cf-5fd8-45ad-a280-67ff30b1071d)


Confirmed GLS output:

![badmux gls](https://github.com/siddharthanand3/vsdhdp/assets/171400217/1d1a7da5-ac43-4a1a-8460-c3a05080d0df)


Here, we see that the GLS output does not match the initial GTKWave simulation. This is a prime example of synthesis simulation mismatch. The cause of this is missing sensitivity list. As we see in the verilog code, the 'if' loop is dependent solely on the changing nature of the 'sel' input. This doesn't take into account the changes occuring in 'i0' and 'i1' inputs. 


Blocking_caveat.v:



File:

![blockingcaveatfile](https://github.com/siddharthanand3/vsdhdp/assets/171400217/59ce738b-d7ef-4cbf-ad11-920aafc23f49)


GTKWave simulatiom:

![blockingcaveat gtk](https://github.com/siddharthanand3/vsdhdp/assets/171400217/25cf96a8-6956-48c6-93b9-bcd72315d68e)


Netlist:

![blocking caveat netlist](https://github.com/siddharthanand3/vsdhdp/assets/171400217/362f6518-6409-41b2-bf9a-834f71717220)


Confirmed GLS output:

![gls output blocking caveat](https://github.com/siddharthanand3/vsdhdp/assets/171400217/14045d2f-88ae-4cf4-970d-18ec67712e25)


In the above example, the Gate Level Simulation failed. This is due to a synthesis simulation mismatch caused by a blocking statement. Hence, it is important to note that we need to avoid using blocking statements as much as possible. And when we do use them, we need to have the utmost clarity so as to prevent such errors.


Week 3


Compilation of the C code:

Installation of the leafpad editor:

sudo apt install leafpad


Open the leafpad editor:

1) leafpad (name of the file).c


C code:

![sum1ton c ss](https://github.com/siddharthanand3/vsdhdp/assets/171400217/30c5058a-cf2c-456a-8fab-f3f53502b2a2)


Output of the code:

2) gcc (name of the file).c
3) ls -ltr
4) ./a.out

![Screenshot 2024-06-25 180104](https://github.com/siddharthanand3/vsdhdp/assets/171400217/1296a5cf-bf2f-4662-8d56-9af62cb1de26)



RISC - V based lab:


Code:

Display the C code on terminal:

1) cat (name of the file).c![cat c code](https://github.com/siddharthanand3/vsdhdp/assets/171400217/77a956b2-28ec-437f-85cb-821a7be989d6)


Utilising the RISC V compiler:

2) riscv64-unknown-elf-gcc -o1 -mabi=lp64 -march=rv64i -o (name of the file).o (name of the file).c

Check if the file has been created:

3) ls -ltr (name of the file).o
   
![Screenshot 2024-06-25 185502](https://github.com/siddharthanand3/vsdhdp/assets/171400217/7fc1a23f-a8fa-47a2-9d5a-60415d9b0f85)


In order to view the Assembly level breakdown:

4) riscv64-unknown-elf-objdump -d (name of the C file).o
5) riscv64-unknown-elf-objdump -d (name of the C file).o | less
6) /main

![main objdump](https://github.com/siddharthanand3/vsdhdp/assets/171400217/ad410aa2-850e-4c78-997a-11e44f521456)


