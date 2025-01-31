## Event Generation
This module allows you to perform event generation using the **`Fun4Sim.C`** macro, similar to the [SimChainDev module](https://github.com/E1039-Collaboration/e1039-analysis/tree/master/SimChainDev). The macro uses the `Fun4AllRUSEventInputManager` and `Fun4AllRUSEventOutputManager` for RUS file input and output handling.

The basic file structure is defined to include only the minimum or most significant variables needed for further processing, such as reconstruction and vertexing.

## Event-Level Variables
| Variable Name      | Type               | Description                          |
|--------------------|--------------------|--------------------------------------|
| `runID`            | `int`              | Identifier for the current run       |
| `spillID`          | `int`              | Identifier for the spill in the run  |
| `eventID`          | `int`              | Unique identifier for the event      |
| `turnID`           | `int`              | Identifier for the Turn              |
| `rfID`             | `int`              | Identifier for the RF                |
| `fpgaTrigger`      | `int[5]`           | Array of FPGA trigger	         |
| `nimTrigger`       | `int[5]`           | Array of NIM trigger	         |
| `rfIntensity`      | `int[33]`          | Array for QIE RF intensities         |


## Hit-Level Variables
| Variable Name          | Type                     | Description                                  |
|------------------------|--------------------------|----------------------------------------------|
| `hitID`                | `std::vector<int>`       | Hit IDs for all hits                         |
| `detectorID`           | `std::vector<int>`       | Detector IDs for all hits                    |
| `elementID`            | `std::vector<int>`       | Element IDs associated with each hit         |
| `driftDistance`        | `std::vector<double>`    | Drift distances for each hit                 |
| `tdcTime`              | `std::vector<double>`    | TDC timing values for each hit               |

# Simulation Guide


1. Access the [Rivanna computer](https://ood.hpc.virginia.edu/pun/sys/dashboard).
2. Clone the repository:
   ```bash
   git clone https://github.com/uva-spin/UVA_RUS_Basic
   ```
3. Go to the repository:
   ```bash
   cd UVA_RUS_Basic
   ```
4. Set up the environment:
   ```bash
    source /project/ptgroup/spinquest/this-e1039.sh
   ```

5. This repository contains the simulation macros and code to test and run the Fun4Sim simulation locally. Run the simulation macro locally for testing. In the `Fun4Sim.C` macro, we have used: 

```cpp
const bool count_only_good_events = false; 
se->run(nevent, count_only_good_events);

This means that the Fun4All macro will keep running until we get accepted events, as required by the SQGeomAcc condition.You can use the package based on your interest. For example, let's use Drell-Yan events, where the beam interaction point is at the target location:

   ```bash
   cd DY_Target
   root -b 'Fun4Sim.C(10)'
   ```

6. Once the job runs locally and looks alright, you can submit a few jobs on the grid before submitting large jobs:
   ```bash
   ./jobscript.sh test 2 10
   ```
   - `test` is the name of this job, used as the name of a new directory to store job outputs.
   - `2` is the number of jobs.
   - `10` is the number of accepted events per job.
   - Job outputs will appear under `/sfs/weka/scratch/<username>/MC`.
   - You can use "squeue -u user", to check the status of your jobs (or use the "Active Jobs" tab on your UVA OpenOnDemand [web page:](https://ood.hpc.virginia.edu/pun/sys/dashboard/activejobs?jobcluster=all&jobfilter=user)).
   
7. Once everything looks alright, submit large jobs. For example:
   ```bash
   ./jobscript.sh DY_Target 100 1000
   ```
8. For more detailed information regarding job subscriptions, read the instructions here: [SpinQuest Monte Carlo Generation on Rivanna](https://confluence.admin.virginia.edu/display/twist/SpinQuest+Monte+Carlo+Generation+on+Rivanna).


In the Fun4Sim.C macro, we have used:

```cpp
const bool count_only_good_events = false;
se->run(nevent, count_only_good_events);
