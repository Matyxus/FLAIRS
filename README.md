<div id="top"></div>


<!-- PROJECT LOGO -->
<br />
<div align="center">
  <h3 align="center">Urban Traffic Control</h3>

  <p align="center">
    Bachelor thesis project: connecting automated planning with SUMO simulator
  </p>
</div>



<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
   <li>
      <a href="#scenario">Traffic Scenario</a>
    </li>
    <li>
      <a href="#usafe">Usage</a>
      <ul>
        <li><a href="#description">Description</a></li>
        <li><a href="#classes">Classes</a></li>
        <li><a href="#example">Example</a></li>
      </ul>
    </li>
  </ol>
</details>



<!-- ABOUT THE PROJECT -->
## About The Project



Project description.

<p align="right">(<a href="#top">back to top</a>)</p>



### Built With

* [gcc]()
* [g++]()
* [python3.9]()
* [Cmake]() (for planner installation)
* [Make]() (for planner installation)

Python libraries: (version can be found in requirements.txt file)
* [pip](https://pypi.org/project/pip/)
* [wheel](https://pypi.org/project/wheel/)
* [setuptools](https://pypi.org/project/setuptools/)
* [numpy](https://numpy.org/)
* [matplotlib](https://matplotlib.org/)
* [prompt_toolkit](https://python-prompt-toolkit.readthedocs.io/en/master/)
* [traci](https://pypi.org/project/traci/)
* [sumolib](https://pypi.org/project/sumolib/)

<p align="right">(<a href="#top">back to top</a>)</p>



<!-- GETTING STARTED -->
## Getting Started

This is an example of how you may give instructions on setting up your project locally.
To get a local copy up and running follow these simple example steps.

### Prerequisites


1) [osmfilter.c](https://wiki.openstreetmap.org/wiki/Osmfilter) (located in UTC/utc/data/osm_filter) has to be compiled
  ```sh
  gcc osmfilter.c -O3 -o osmfilter
  ```
2) [SUMO](https://www.eclipse.org/sumo/) has to be downloaded and installed
3) [Planners](https://ipc2018-classical.bitbucket.io/#description) 
used in this project: [MERWIN](https://bitbucket.org/ipc2018-classical/team14/src/ipc-2018-seq-agl/), 
[Cerberus](https://bitbucket.org/ipc2018-classical/team15/src/ipc-2018-seq-agl/) (agile)

### Installation

Below is an example of how you can instruct your audience on installing and setting up your app. This template doesn't rely on any external dependencies or services._

1. Clone the repo
   ```sh
   git clone https://github.com/Matyxus/UTC
   ```
2. In folder where UTC is located execute:
   ```sh
   pip install -e UTC
   ```
3. Install requirements (in ../UTC)
   ```sh
   pip install -r requirements.txt
   ```
4. Compile planners (expected [IPC](https://www.icaps-conference.org/competitions/) 
standard as input -> domain, problem file, result file)
5. Give permission to project enabling it to create, delete: folders, files and execute
commands with [subprocess](https://docs.python.org/3/library/subprocess.html) library.
6. Execute UTC/utc/test/test_launcher.py for testing the whole project

<p align="right">(<a href="#top">back to top</a>)</p>

<!-- Scenario -->
## Traffic Scenario
Scenarios consist of following files:
1. ".sumocfg" file (which runs the simulation in SUMO GUI)
2. ".rou.xml" file (contains vehicle types, routes, vehicles)
3. ".info" file (contains the commands used to generate scenario)
4. Optional files:  
   1. ".stat.xml" file (contains vehicle statistics, average values of speed, delay, etc.)
   2. ".pddl" problem file (contains the scenario represented in pddl format)
   3. ".pddl" result file (contains the action of objects from pddl problem files, generated by planner) 
   4. ".net.xml" network file (subgraph of network made specifically for this scenario)

Scenarios are always automatically created with launcher (".sumocfg") and routes file (".rou.xml").\
There are two types of scenarios:
1. user generated (created by using commands in [ScenarioMain](./utc/src/simulator) class)
2. planner generated (commands in [UtcLauncher](./utc/src/pddl/utc_problem) class), which can only be used on already existing scenarios\
Scenarios structure is following (located in [data folder](./utc/data/scenarios)):
   ```
   [scenario_name]
      |
      └─── maps* (folder related to subgraphs)
      │      └─── information (".info" files logging how subgraph was generated)
      │      └─── networks    (".net.xml" subgraphs)
      └─── problems* (folder containing pddl problems)
      │       └─── [scenario] (folder of pddl problems generated from "[scenario].sumocfg")
      └─── results* (folder containing result of planner)
      │       └─── [scenario] (folder of pddl results generated from "[scenario].sumocfg")
      └─── routes ("[scenario].rou.xml" files, corresponding to "[scenario].sumocfg")  
      └─── simulation (folder )
               └─── config ("[scenario].sumocfg" files, launching simulation)
               └─── statistics* ("[scenario].stat.xml" files containing statistics of simulation)
               └─── information ("[scenario].info" files logging how simulation was generated)

   Note: [scenario] is name chosen by user (when generating
   planned scenario, new [scenario] name is inputted by user, all
   ".sumocfg" files must have unique names), their names are used as "name-space",
   refferencing to similary named files e.g. ".rou.xml", ".stat.xml", etc...
   * Folders denoted by star (*) are created only when their content is created.
   ```
<p align="right">(<a href="#top">back to top</a>)</p>

<!-- USAGE EXAMPLES -->
## Usage



### Description
Classes can be either run dynamically:
   ```sh
   python3 converter.py
   ```
which on execution prompts user for input. At first user has to input name of command
to be executed, followed by filling of required arguments between "quotes" (if there are any).
[Prompt_toolkit](https://python-prompt-toolkit.readthedocs.io/en/master/) library is used
to ask for input, which offers auto-completion when asking for command names, and
history of used command names and arguments.
![Input Example](Images/ui_input_example.PNG)
All classes have by default commands:
1. **_help_** -> which prints all commands, their parameters and their description
2. **_exit_** -> which quits the program.

Another option is to launch program statically by redirecting files as input:
   ```sh
   python3 converter.py < file_name.extension
   ```
Such files have exactly one command and its arguments per line, e.g.:
 ```
 convert map_name="Berlin"
 convert map_name="London"
 exit *(does not need to be present)
 ```

### Classes

There are 5 classes that can be launched:
1. [Converter](./utc/src/converter) responsible for conversion of ".osm" files to ".net.xml"
2. [GraphMain](./utc/src/graph) responsible for graph manipulation (creation of sub-graphs)
3. [ScenarioMain](./utc/src/simulator) responsible for generation scenarios (".sumocfg" files)
4. [UtcLauncher](./utc/src/pddl/utc_problem) responsible for planning scenarios
5. [Main](./utc/src) combining all the above into single class

<details>
  <summary>Converter</summary>
  Converter class converts downloaded ".osm" files into ".net.xml" files which SUMO recognizes.
  It does to by filtering out all non-highway related objects (except traffic semaphores),
  creating same named file (with the suffix "_filtered" added). Filtered file
  is then used to convert into ".net.xml" by running netconvert (part of SUMO commands)
  in command shell, which generates ".net.xml" file with the same name as the original ".osm" file.
</details>

<details>
  <summary>GraphMain</summary>
  GraphMain class can display ".net.xml" files using pyplot (also shows junction id's),
  generate sub-graphs (using Top K A* algorithm, which limits the number of routes
  by parameter "c" -> which is used to multiply shortest route length from given starting
  and ending junctions, thereby limiting route length). Subsequently sub-graphs can be
  merged together, allowing to create much smaller graphs that focus only on 
  parts of network where cars are driving on. Finally sub-graphs created like this
  can be saved, creating new ".net.xml" file.
</details>

<details>
  <summary>ScenarioMain</summary>
  ScenarioMain class allows user to create custom scenarios on given road network.
  User can select commands to add vehicle / vehicle-flows into network from
  given starting and ending junction (route is selected by shortest path).
  When saving scenarios, 3 files are created: ".sumocfg", ".rou.xml", ".info",
  having the same name as user gave to "generate-scenario" command argument "scenario_name".
  Scenarios can be launched with or without GUI and similarly user can generate file
  containing vehicle statistics (".stat.xml").
</details>

<details>
  <summary>UtcLauncher</summary>
  UtcLauncher class handles files with ".pddl" extension.
  User has to at first select already existing scenario (created by ScenarioMain),
  and name for new scenario (which is used for pddl problem/result folder names and 
  prefix of such files), network on which user wants the planner to plan vehicle
  routes on (sub-graph of original network is recommended, since the larger the network, 
  the more time is required for planner to find solution).
  After initialization is finished, user can generate problem files, describing
  the simulation in ".pddl" format in given time frame (e.g. from 0-20 step, 20-40, ...).
  If problem files are generated, it is possible to generate result files (at least 
  30 seconds of planner execution time is recommend for planning).
  Finally it is possible to generate "planned" scenario from ".pddl" result files (this 
  is so called "offline" planning), it is possible to plan while the simulation is running
  ("online" planning), where every X time-step simulation is paused to generate corresponding
  problem file and result file from which vehicle routes are extracted and given to 
  vehicles in simulation (the simulation then continues to run until next X-th time-step
  when the process repeats until the end).
</details>

### Example

1. Download maps from [OpenStreetMap](https://www.openstreetmap.org/). \
Save them in [UTC/utc/data/maps/osm](./utc/data/maps/osm) folder.
2. Convert downloaded maps into '.net.xml' format using [Converter](./utc/src/converter).
3. Generate scenario using [ScenarioMain](./utc/src/simulator). 
4. (Optional) Generate sub-graph for road network used in scenario using [GraphMain](./utc/src/graph). \
Planners can take long time to generate solutions if road network (or number of cars)
is large, for that reason it is advantageous to use smaller networks (sub-graphs).
5. Generate planned scenario using [UtcLauncher](./utc/src/pddl/utc_problem).
6. Launch scenario and compare vehicle statistics against planned simulation using [ScenarioMain](./utc/src/simulator).

<p align="right">(<a href="#top">back to top</a>)</p>
