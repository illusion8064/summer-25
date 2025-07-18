```mermaid
flowchart TD
    %% CVA6 Core
    subgraph "CVA6 Core"
        direction TB
        subgraph "Pipeline Stages"
            direction TB
            IF["IF Stage<br/>(frontend.sv)"]:::pipeline
            ID["ID Stage<br/>(id_stage.sv)"]:::pipeline
            EX["EX Stage<br/>(ex_stage.sv)"]:::pipeline
            MEM_STAGE["MEM Stage<br/>(load_store_unit.sv)"]:::pipeline
            WB["Commit Stage<br/>(commit_stage.sv)"]:::pipeline
        end
        IF -->|fetch| ID -->|decode| EX -->|execute| MEM_STAGE -->|commit| WB

        subgraph "Memory Subsystem"
            direction TB
            ICache["ICache"]:::memory
            DCache["DCache<br/>(core/cache_subsystem/)"]:::memory
            MMU["MMU/TLB<br/>(cva6_mmu.sv)"]:::memory
        end
        MEM_STAGE -->|load/store| DCache
        DCache -->|AXI transactions| AXI

        subgraph "Interfaces & Buses"
            direction TB
            AXI["AXI Shim<br/>(axi_shim.sv)"]:::bus
            APB["APB Bridge"]:::bus
        end
        WB -->|RVFI trace| RVFI
        DCache --> AXI
        AXI --> APB

        subgraph "Peripherals"
            direction TB
            BootROM["Boot ROM<br/>(bootrom.sv)"]:::peripheral
            CLINT["CLINT<br/>(clint.sv)"]:::peripheral
            PLIC["PLIC<br/>(rv_plic/)"]:::peripheral
            UART["UART"]:::peripheral
        end
        APB --> BootROM
        APB --> CLINT
        APB --> PLIC
        APB --> UART
    end

    subgraph "APU & SoC Integration"
        direction TB
        SoC["SoC Wrapper<br/>(ariane.sv)"]:::bus
    end
    AXI --> SoC

    subgraph "Verification & Simulation"
        direction TB
        TB["Testbench Harness<br/>(ariane_testharness.sv)"]:::verification
        UVM["UVM Env & Agents<br/>(verif/env/uvme/)"]:::verification
        Spike["Spike Comparator"]:::external
        Python["Python Orchestration<br/>(cva6.py)"]:::external
    end
    SoC --> TB
    TB --> UVM
    WB --> RVFI["RVFI Trace<br/>(cva6_rvfi.sv)"]:::external
    RVFI --> UVM
    UVM --> Spike
    UVM --> Python

    subgraph "ASIC Flow"
        direction TB
        Synth["Synth Scripts<br/>(pd/synth/cva6_synth.tcl)"]:::external
        GateAnal["Gate Analysis"]:::external
    end
    SoC --> Synth

    subgraph "Performance Model"
        direction TB
        Perf["Perf Model<br/>(model.py)"]:::external
    end
    SoC --> Perf

    subgraph "External Tools"
        direction TB
        VerilatorCloud["Verilator"]:::external
        VCSCloud["VCS"]:::external
        Quartus["Quartus"]:::external
        Vivado["Vivado"]:::external
    end
    TB --> VerilatorCloud
    TB --> VCSCloud
    SoC --> Quartus
    SoC --> Vivado

    %% Click Events
    click IF "https://github.com/openhwgroup/cva6/blob/master/core/frontend/frontend.sv"
    click ID "https://github.com/openhwgroup/cva6/blob/master/core/id_stage.sv"
    click EX "https://github.com/openhwgroup/cva6/blob/master/core/ex_stage.sv"
    click MEM_STAGE "https://github.com/openhwgroup/cva6/blob/master/core/load_store_unit.sv"
    click WB "https://github.com/openhwgroup/cva6/blob/master/core/commit_stage.sv"
    click DCache "https://github.com/openhwgroup/cva6/tree/master/core/cache_subsystem/"
    click MMU "https://github.com/openhwgroup/cva6/blob/master/core/cva6_mmu/cva6_mmu.sv"
    click AXI "https://github.com/openhwgroup/cva6/blob/master/core/axi_shim.sv"
    click BootROM "https://github.com/openhwgroup/cva6/blob/master/corev_apu/bootrom/bootrom.sv"
    click CLINT "https://github.com/openhwgroup/cva6/blob/master/corev_apu/clint/clint.sv"
    click PLIC "https://github.com/openhwgroup/cva6/tree/master/corev_apu/rv_plic/"
    click SoC "https://github.com/openhwgroup/cva6/blob/master/corev_apu/src/ariane.sv"
    click TB "https://github.com/openhwgroup/cva6/blob/master/corev_apu/tb/ariane_testharness.sv"
    click UVM "https://github.com/openhwgroup/cva6/tree/master/verif/env/uvme/"
    click RVFI "https://github.com/openhwgroup/cva6/blob/master/core/cva6_rvfi.sv"
    click Synth "https://github.com/openhwgroup/cva6/blob/master/pd/synth/cva6_synth.tcl"
    click Perf "https://github.com/openhwgroup/cva6/blob/master/perf-model/model.py"

    %% Styles
    classDef pipeline fill:#ADD8E6,stroke:#333,stroke-width:1px
    classDef memory fill:#FFA500,stroke:#333,stroke-width:1px
    classDef bus fill:#90EE90,stroke:#333,stroke-width:1px
    classDef peripheral fill:#FFFF99,stroke:#333,stroke-width:1px
    classDef verification fill:#D3D3D3,stroke:#333,stroke-width:1px
    classDef external fill:#CCCCFF,stroke:#333,stroke-width:1px
```