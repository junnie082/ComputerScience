# System Structure

# Chapter 1 Computer Abstractions and Technology

#### 2024.09.23.(월)

### The Computer Revolution

- Progress in computer technology

  - Underpinned by domain-specific accelerators

- Makes novel applications feasible

  - Computers in automobiles
  - Cell phones
  - Human genome project
  - World Wide Web
  - Search Engines

- Computers are pervasive

### Classes of Computers

- Personal computers

  - General purpose, variety of software
  - Subject to cost/performance tradeoff

- Server computers

  - Network based
  - High capacity, performance, reliability
  - Range from small servers to building sized

- Supercomputers

  - Type of server
  - High-end scientific and engineering calculations
  - Highest capability but represent a small fraction of the overall computer market

- Embedded computers
  - Hidden as components of systems
  - Stringent power/performance/cost constraints

### The PostPC Era

- Personal Mobile Device (PMD)

  - Battery operated
  - Connects to the Internet
  - Hundreds of dollars
  - Smart phones, tablets, electronic glasses

- Cloud computing
  - Warehouse Scale Computers (WSC)
  - Software as a Service (SaaS)
  - Portion of software run on a PMD and a portion run in the Cloud
  - Amazon and Google

### What You Will Learn

- How programs are translated into the machine language

  - And how the hardware executes them

- The hardware/software interface

- What determines program performance

  - And how it can be improved

- How hardware designers improve performance

- What is parallel processing

### Understanding Performance

- Algorithm

  - Determines number of operations executed

- Programming language, compiler, architecture

  - Determine number of machine instructions executed per operation

- Processor and memory system

  - Determine how fast instructions are executed

- I/O system (including OS)
  - Determines how fast I/O operations are executed

### Seven Great Ideas

- Use `abstraction` to simplify design

- Make the `common case fast`

- Performance via `parallelism`

- Performance via `pipelining`

- Performance via `prediction`

- `Hierarchy` of memories

- `Dependability` via redundancy

### Below Your Program

- Application software

  - Written in high-level language

- System software

  - Compiler: translates HLL code to machine code
  - Operating System: service code
    - Handling input/output
    - Managing memory and storage
    - Scheduling tasks & sharing resources

- Hardware
  - Processor, memory, I/O controllers

### Levels of Program Code

- High-level language

  - Level of abstraction closer to problem domain
  - Provides for productivity and portability

- Assembly language

  - Textual representation of instructions

- Hardware representation
  - Binary digits (bits)
  - Encoded instructions and data

### Components of a Computer

- Same components for all kinds of computer

  - Desktop, server, embedded

- Input/output includes

  - User-interface devices

    - Display, keyboard, mouse

  - Storage devices

    - Hard disk, CD/DVD, flash

  - Network adapters
    - For communicating with other computers

### Touchscreen

- PostPC device

- Supersedes keyboard and mouse

- Resistive and Capacitive types
  - Most tablets, smart phones use capacitive
  - Capacitive allows multiple touches simultaneously

### Through the Looking Glass

- LCD screen: picture elements (pixels)
  - Mirrors content of frame buffer memory

### Inside the Processor (CPU)

- Datapath: performs operations on data

- Control: sequences datapath, memory, ...

- Cache memory

  - Small fast SRAM memory for immediate access to data

- A12 processor

### Abstractions

- Abstraction helps us deal with complexity

  - Hide lower-level detail

- Instruction set architecture (ISA)

  - The hardware/software interface

- Application binary interface

  - The ISA plus system software interface

- Implementation
  - The details underlying and interface

### A Safe Place for Data

- Volatile main memory

  - Loses instructions and data when power off

- Non-volatile secondary memory
  - Magnetic disk
  - Flash memory
  - Optical disk (CDROM, DVD)

### Networks

- Communication, resource sharing, nonlocal access

- Local area network (LAN): Ethernet

- Wide area network (WAN): the Internet

- Wireless network: WiFi, Bluetooth

### Technology Trends

- Electronics technology continues to evolve
  - Increased capacity and performance
  - Reduced cost

| Year | Technology | Relative performance/cost |
| 1951 | Vacuum tube | 1 |
| 1965 | Transistor | 35 |
| 1975 | Integrated circuit (IC) | 900 |
| 1995 | Very large scale IC (VLSI) | 2,400,000 |
| 2013 | Ultra large scale IC | 250,000,000,000 |

### Semiconductor Technology

- Silicon: semiconductor

- Add materials to transform properties:
  - Conductors
  - Insulators
  - Switch

### Manufacturing ICs

- Yield: proportion of working dies per wafer

### Intel Core 10th Gen

- 300mm wafer, 506 chips, 10nm technology

- Each chip is 11.4 x 10.7 mm

### Integrated Circuit Cost

```
Cost per die = Cost per wafer / Dies per wafer x Yield

Dies per wafer = Wafer area / Die area

Yield = 1 / (1 + (Defectsper area x Die area/2)^2)^2

```

- Nonlinear relation to area and defect rate
  - Wafer cost and area are fixed
  - Defect rate determined by manufacturing process
  - Die area determined by architecture and circuit design

### Response Time and Throughput

- Response time

  - How long it takes to do a task

- Throughput

  - Total work done per unit time
    - e.g., tasks/transactions/... per hour

- How are response time and throughput affected by

  - Replacing the processor with a faster version?
  - Adding more processors?

- We'll focus on response time for now...

### Relative Performance

- Define Performance = 1/Execution Time

- "X is n time faster than Y"

```
Performance X / Performance Y = Execution time Y / Execution time X = n
```

- Example: time taken to run a program
  - 10s on A, 15s on B
  - Execution Time B / Execution Time A = 15s / 10s = 1.5
  - So A is 1.5 times faster than B

### Measuring Execution Time

- Elapsed time

  - Total response time, including all aspects
    - Processing, I/O, OS overhead, idle time
  - Determines system performance

- CPU time
  - Time spent processing a given job
    - Discounts I/O time, other jobs' shares
  - Comprises user CPU time and system CPU time
  - Different programs are affected differently by CPU and system performance

### CPU Clocking

- Operation of digital hardware governed by a constant-rate clock

- Clock period: duration of a clock cycle

  - e.g., 250ps = 0.25ns = 250x10^-12s

- Clock frequency (rate): cycles per second
  - e.g., 4.0GHz = 4000MHz = 4.0x10^9Hz

### CPU Time

```
CPU Time = CPU Clock Cycles x Clock Cycle Time
= CPU Clock Cycles / Clock Rate
```

- Performance improved by
  - Reducing number of clock cycles
  - Increasing clock rate
  - Hardware designer must often trade off clock rate against cycle count

### CPU Time Example

- Computer A: 2GHz clock, 10s CPU time
- Designing Computer B
  - Aim for 6s CPU time
  - Can do faster clock, but causes 1.2 x clock cycles
- How fast must Computer B clock be?

```
Clock Rate B = Clock Cycles B / CPU Time B = 1.2 x Clock Cycles A / 6s

Clock Cycles A = CPU Time A x Clock Rate A = 10s x 2GHz = 20 x 10^9

Clock Rate B = 1.2 x 20 x 10^9 / 6s = 24 x 10^9 / 6s = 4 GHz
```

### Instruction Count and CPI

``` 
Clock Cycles = Instruction Count x Cycles per Instruction

CPU Time = Instruction Count x CPI x Clock Cycle Time
= Instruction Count x CPI / Clock Rate
```

- Instruction Count for a program

  - Determined by program, ISA and compiler

- Average cycles per instruction
  - Determined by CPU hardware
  - If different instructions have different CPI
    - Average CPI affected by instruction mix

### CPI Example

- Computer A: Cycle Time = 250ps, CPI = 2.0
- Computer B: Cycle Time = 500ps, CPI = 1.2
- Same ISA
- Which is faster, and by how much?

```
CPU Time A = Instruction Count x CPI A x Cycle Time A = I x 2.0 x 250ps = I x 500ps <- A is faster...

CPU Time B = Instruction Count x CPI B x Cycle Time B = I x 1.2 x 500ps = I x 600ps

CPU Time B / CPU Time A = I x 600ps / I x 500ps = 1.2 <- ...by this much
```

### CPI in More Detail

- If different instruction classes take different numbers of cycles

`Clock Cycles = sum from i = 1 to i = n (CPIi x Instruction Counti)`

- Weighted average CPI
  `CPI = Clock Cycles / Instruction Count = sum from i = 1 to i = n (CPIi x Instruction Counti / Instruction Count)`

### CPI Example

- Alternative compiled code sequences using instructions in classes A, B, C

| Class | A | B | C |
| CPI for class | 1 | 2 | 3 |
| IC in sequence 1 | 2 | 1 | 2 |
| IC in sequence 2 | 4 | 1 | 1 |

- Sequence 1: IC = 5

  - Clock Cycles
    = 2x1 + 1x2 + 2x3
    = 10
  - Avg. CPI = 10/5 = 2.0

- Sequence 2: IC = 6
  - Clock Cycles
    = 4x1 + 1x2 + 1x3
    = 9
  - Avg. CPI = 9/6 = 1.5

### Performance Summary

``` 
CPU Time = Instructions/Program x Clock cycles/Instruction n x Seconds/Clock cycle
```

- Performance depends on
  - Algorithm: affects IC, possibly CPI
  - Programming language: affects IC, CPI
  - Compiler: affects IC, CPI
  - Instruction set architecture: affects IC, CPI, Tc

### Power Trends

- In CMOS IC technology

`Power = Capacitive laod x Voltage^2 x Frequency`

### Reducing Power

- Suppose a new CPU has

  - 85% of capacitive load of old CPU
  - 15% voltage and 15% frequency reduction

  `Pnew / Pold = Cold x 0.85 x (Vold x 0.85)^2 x Fold x 0.85 / (Cold X Vold^2 x Fold) = 0.85^4 = 0.52`

- The power wall

  - We can't reduce voltage further
  - We can't remove more heat

- How else can we improve performance?

### Uniprocessor Performance

-> Constrained by power, instruction-level parallelism, memory latency

### Multiprocessors

- Multicore microprocessors

  - More than one processor per chip

- Requires explicitly parallel programming
  - Compare with instruction level parallelism
    - Hardware executes multiple instructions at once
    - Hidden from the programmer
  - Hard to do
    - Programming for performance
    - Load balancing
    - Optimizing communication and synchronization

### SPEC CPU Benchmark

- Programs used to measure performance

  - Supposedly typical of actual workload

- Standard Performance Evaluation Corp (SPEC)

  - Developers benchmarks for CPU, I/O, Web, ...

- SPEC CPU 2006
  - Elapsed time to execute a selection of programs
    - Negligible I/O, so focuses on CPU performance
  - Normalize relative to reference machine
  - Summarize as geometric mean of performance ratios
    - CINT2006 (integer) and CFP2006 (floating-point)

### SPEC Power Benchmark

- Power consumption of server at different workload levels
  - Performance: ssj_ops/sec
  - Power: Watts (Joules/sec)

`Overallssj_ops per Watt = (sum from i = 0 to i = 10 ssj_ops i) / (sum from i = 0 to i = 10 power i)`

### Pitfall: Amdahl's Law

- Improving an aspect of a computer and expecting a proportional improvement in overall performance

`T improved = T affected / improvement factor + T unaffected

- Example: multiply accounts for 80s / 100s

  - How much improvement in multiply performance to get 5x overall?

  20 = 80/n + 20

  - Can't be done!

- Corollary: make the common case fast

### Fallacy: Low Power at Idle

- Look back at i7 power benchmark

  - At 100% load: 258 W
  - At 50% load: 170 W (66%)
  - At 10% load: 121 W (47%)

- Google data center

  - Mostly operates at 10% - 50% load
  - At 100% load less than 1% of the time

- Consider designing processors to make power proportional to load

### Pitfall: MIPS as a Performance Metric

- MIPS: Millions of Instructions Per Second
  - Doesn't account for
    - Differences in ISAs between computers
    - Differences in complexity between instructions

```
MIPS = Instruction count / Execution time x 10^6

= Instruction count / (Instruction count x CPI x 10^6 / Clock rate) = Clock rate / (CPI x 10^6)
```

- CPI varies between programs on a given CPU

### Concluding Remarks

- Cost/performance is improving

  - Due to underlying technology development

- Hierarchical layers of abstraction

  - In both hardware and software

- Instructioin set architecture

  - The hardware/software interface

- Execution time: the best performance measure

- Power is a limiting factor
  - User parallelism to improve performance
