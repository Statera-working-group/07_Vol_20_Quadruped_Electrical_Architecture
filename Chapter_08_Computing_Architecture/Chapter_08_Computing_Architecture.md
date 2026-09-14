**Volume 20. Quadruped Electrical Architecture**


# Chapter 08. Computing Architecture

##  

## 08.01. Jetson Architecture

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

In a quadruped robot, the Jetson architecture serves as the primary high-performance computing layer for perception, localization, mapping, intelligent navigation, and Physical AI workloads. It complements rather than replaces deterministic motor and safety controllers. The computing structure should therefore separate AI-intensive processing from hard real-time joint control while providing reliable interfaces between both domains.

A Jetson-based computing node combines CPU cores, GPU acceleration, memory, storage, and high-speed I/O within an embedded power envelope suitable for a mobile robot. The CPU typically manages operating-system services, ROS 2 processes, communication, and supervisory logic, while the GPU executes highly parallel workloads such as neural-network inference, image processing, point-cloud processing, and learned perception.

Platform selection must consider the complete mission profile rather than peak AI performance alone. Compute demand depends on the number and resolution of cameras, LiDAR processing rate, localization algorithms, neural-network complexity, planning frequency, and additional inspection workloads. Thermal limits, battery consumption, physical packaging, available interfaces, and future model growth are equally important design parameters.

The Jetson node normally occupies the upper layer of a heterogeneous computing hierarchy. Real-time controllers close fast control loops for BLDC or PMSM actuators, process encoder and torque information, and maintain deterministic communication with joint modules. Jetson receives processed robot states and sensor streams, estimates the environment and robot condition, generates higher-level commands, and transfers motion objectives to the real-time domain.

Sensor connectivity strongly influences Jetson architecture. Cameras may use CSI or Ethernet interfaces, while LiDAR, GNSS, inspection sensors, and external computing devices commonly rely on Ethernet, USB, or dedicated gateways. High-bandwidth sensors should be distributed so that simultaneous traffic does not create interface congestion. Timestamp integrity must also be maintained from sensor acquisition through perception and state estimation.

Memory architecture is particularly important because perception pipelines frequently move large tensors, images, feature maps, and point clouds between processing stages. Excessive copying increases latency and memory bandwidth consumption even when GPU utilization appears moderate. Efficient implementations therefore favor hardware-assisted decoding, GPU-resident processing, shared or zero-copy buffers where appropriate, and carefully controlled transitions between CPU and GPU memory spaces.

Storage must support more than the operating system. A practical quadruped may retain AI models, calibration parameters, maps, mission configurations, diagnostic traces, event logs, and temporary sensor recordings. High-performance local storage can reduce model-loading and logging bottlenecks, while partitions or logically separated storage areas help protect critical software from uncontrolled growth of recorded data.

Power integration requires the Jetson subsystem to be treated as a managed electrical load rather than an ordinary computer. DC-DC conversion should tolerate battery-voltage variation and actuator-induced transients while maintaining stable supply conditions. Startup sequencing, controlled shutdown, undervoltage handling, and power-state monitoring are essential because abrupt power loss can corrupt storage, interrupt logging, or leave mission software in an uncertain state.

Thermal architecture directly determines sustainable computing performance. A Jetson module that meets nominal inference requirements may throttle when installed inside a sealed quadruped body exposed to high ambient temperature or continuous locomotion. Heat spreaders, conduction paths, airflow where practical, temperature telemetry, and workload-aware power modes should therefore be designed together with mechanical packaging rather than added after software development.

The software stack should isolate hardware-specific acceleration from application-level robotics functions. Linux and the Jetson software environment provide device drivers and GPU services, while CUDA-based libraries and inference runtimes accelerate perception and AI models. ROS 2 can then organize cameras, LiDAR, localization, mapping, planning, diagnostics, and mission functions into modular nodes with explicit interfaces and measurable timing behavior.

AI inference should be engineered around end-to-end latency instead of raw throughput alone. A quadruped navigating irregular terrain must convert sensor observations into useful motion decisions within bounded time. Preprocessing, inference, postprocessing, middleware transport, synchronization, and command delivery all contribute to response latency. Profiling should therefore measure complete pipelines under realistic simultaneous sensor and locomotion workloads.

Communication between Jetson and the lower control layer should preserve a clear authority boundary. High-level commands may represent desired body velocity, trajectory, foothold target, gait request, or mission state, whereas joint-level current, torque, position, and protection loops remain under deterministic controllers. This prevents operating-system scheduling or GPU workload variation from directly destabilizing actuator control.

Fault containment is equally important. A Jetson application can fail because of software defects, memory exhaustion, thermal throttling, corrupted sensor input, or overloaded computation. Such failures should not remove fundamental electrical protection or emergency-stop capability. Independent controllers must retain the ability to place actuators into a defined safe state, while watchdogs and health-monitoring mechanisms detect loss of the high-level computing node.

Network architecture should anticipate the bandwidth generated by multiple perception sensors and distributed controllers. Gigabit or higher-speed Ethernet can provide the backbone for cameras, LiDAR, companion computers, and diagnostic equipment, while CAN FD or EtherCAT may serve control-oriented subsystems according to determinism and topology requirements. Traffic prioritization prevents bulk data or logging transfers from interfering with control-relevant communication.

Time synchronization connects the Jetson architecture to the robot\'s physical state. Camera frames, LiDAR scans, IMU samples, encoder states, and actuator feedback must correspond to a consistent temporal reference for accurate sensor fusion and motion estimation. Hardware timestamps and PTP-based synchronization can reduce uncertainty, while software should preserve timestamp provenance instead of replacing acquisition time with later processing time.

Boot and recovery behavior should be designed as part of system architecture. After power application, critical interfaces, time synchronization, sensor drivers, localization, AI services, and mission software should start in a controlled dependency sequence. Supervisory logic must distinguish between components that are merely initializing and components that have failed, preventing locomotion until required computing and sensing services reach validated operational states.

Diagnostics should expose CPU utilization, GPU utilization, memory pressure, storage health, temperatures, power state, network statistics, inference latency, sensor freshness, and process status. These signals allow the robot to distinguish a perception failure from a communication or thermal problem. Historical telemetry also supports field debugging and predictive maintenance when intermittent failures cannot be reproduced during laboratory inspection.

The architecture should accommodate software updates without making recovery dependent on a successful update. Versioned AI models, configuration control, rollback capability, integrity verification, and protected system partitions reduce the risk of remote deployment. Because quadrupeds may operate away from engineering personnel, recovery paths should remain available even when application software, an AI model, or a network configuration becomes invalid.

Jetson scalability also supports separation between baseline and advanced robot configurations. A lower-power platform may execute essential perception and navigation, while a higher-performance configuration can support multiple neural networks, dense 3D perception, VLM-based interpretation, or sophisticated inspection analytics. Maintaining common interfaces across these configurations reduces software fragmentation and preserves portability as computing hardware evolves.

For advanced Physical AI, Jetson becomes the bridge between semantic intelligence and physical execution. Perception models interpret the environment, learned representations provide contextual understanding, and planning components convert that understanding into objectives compatible with locomotion control. The architecture must nevertheless maintain the distinction between probabilistic AI outputs and deterministic constraints imposed by safety, stability, actuator limits, and system health.

A well-designed Jetson architecture is therefore not defined simply by installing a powerful GPU module in the robot. It is an integrated computing subsystem combining AI acceleration, sensor interfaces, deterministic-controller coordination, networking, synchronized time, power management, thermal engineering, diagnostics, and fault containment. This structure provides the computational foundation for the later edge GPU, real-time control, AI acceleration, PTP synchronization, and compute-redundancy topics defined in the computing architecture chapter.

사족보행 로봇(Quadruped Robot)에서 젯슨 아키텍처(Jetson Architecture)는 인지(Perception), 위치추정(Localization), 지도작성(Mapping), 지능형 내비게이션(Intelligent Navigation), 피지컬 AI(Physical AI) 워크로드를 담당하는 주요 고성능 컴퓨팅 계층(High-Performance Computing Layer)으로 동작한다. 이는 결정론적 모터 제어기(Deterministic Motor Controller)와 안전 제어기(Safety Controller)를 대체하는 것이 아니라 상호 보완한다. 따라서 컴퓨팅 구조는 AI 중심 처리와 하드 실시간 관절 제어(Hard Real-Time Joint Control)를 분리하면서 두 영역 사이에 신뢰성 높은 인터페이스를 제공해야 한다.

젯슨 기반 컴퓨팅 노드(Jetson-Based Computing Node)는 CPU 코어(CPU Core), GPU 가속(GPU Acceleration), 메모리(Memory), 저장장치(Storage), 고속 입출력(High-Speed I/O)을 이동형 로봇에 적합한 전력 범위 내에서 통합한다. CPU는 일반적으로 운영체제 서비스, ROS 2 프로세스, 통신 및 상위 감독 로직(Supervisory Logic)을 관리하며, GPU는 신경망 추론(Neural-Network Inference), 영상 처리(Image Processing), 포인트 클라우드 처리(Point-Cloud Processing), 학습 기반 인지(Learned Perception)와 같은 고도의 병렬 워크로드를 실행한다.

플랫폼 선정(Platform Selection)은 단순한 최대 AI 성능이 아니라 전체 임무 프로파일(Mission Profile)을 고려해야 한다. 연산 요구량은 카메라의 수와 해상도, 라이다(LiDAR) 처리 주기, 위치추정 알고리즘, 신경망 복잡도, 경로 계획 주기 및 추가 검사 워크로드에 따라 달라진다. 열적 한계(Thermal Limit), 배터리 소비, 물리적 패키징, 사용 가능한 인터페이스와 향후 모델 확장성 역시 중요한 설계 변수이다.

젯슨 노드(Jetson Node)는 일반적으로 이기종 컴퓨팅 계층(Heterogeneous Computing Hierarchy)의 상위 계층에 위치한다. 실시간 제어기(Real-Time Controller)는 BLDC 또는 PMSM 액추에이터의 고속 제어 루프를 폐루프 제어하고, 엔코더와 토크 정보를 처리하며, 관절 모듈과 결정론적 통신을 유지한다. 젯슨은 처리된 로봇 상태와 센서 스트림을 수신하여 환경과 로봇 상태를 추정하고 상위 수준 명령을 생성한 뒤 이를 실시간 제어 영역으로 전달한다.

센서 연결성(Sensor Connectivity)은 젯슨 아키텍처 구성에 큰 영향을 미친다. 카메라는 CSI 또는 이더넷(Ethernet) 인터페이스를 사용할 수 있으며, 라이다, GNSS, 검사 센서 및 외부 컴퓨팅 장치는 일반적으로 이더넷, USB 또는 전용 게이트웨이(Dedicated Gateway)를 이용한다. 고대역폭 센서의 동시 데이터 전송이 인터페이스 병목을 발생시키지 않도록 분산해야 하며, 센서 취득부터 인지와 상태 추정까지 타임스탬프 무결성(Timestamp Integrity)을 유지해야 한다.

메모리 아키텍처(Memory Architecture)는 인지 파이프라인에서 대규모 텐서(Tensor), 영상, 특징 맵(Feature Map), 포인트 클라우드를 처리 단계 사이에서 빈번하게 이동시키기 때문에 특히 중요하다. 과도한 데이터 복사는 GPU 사용률이 높지 않은 상황에서도 지연시간과 메모리 대역폭 소비를 증가시킨다. 따라서 하드웨어 가속 디코딩, GPU 상주 처리(GPU-Resident Processing), 적절한 공유 또는 제로카피 버퍼(Zero-Copy Buffer), CPU와 GPU 메모리 공간 사이의 신중한 데이터 전환이 필요하다.

저장장치(Storage)는 운영체제만을 위한 공간이 아니다. 실제 사족보행 로봇은 AI 모델, 캘리브레이션 파라미터(Calibration Parameter), 지도, 임무 설정, 진단 추적 데이터(Diagnostic Trace), 이벤트 로그 및 임시 센서 기록을 저장할 수 있다. 고성능 로컬 저장장치는 모델 로딩과 로깅 병목을 줄일 수 있으며, 파티션 또는 논리적으로 분리된 저장 영역은 기록 데이터의 무제한 증가로부터 핵심 소프트웨어를 보호하는 데 도움이 된다.

전원 통합(Power Integration)에서는 젯슨 서브시스템을 일반적인 컴퓨터가 아니라 관리되는 전기 부하(Managed Electrical Load)로 취급해야 한다. DC-DC 변환기는 배터리 전압 변동과 액추에이터에 의해 발생하는 과도현상(Transient)을 견디면서 안정적인 전원을 공급해야 한다. 기동 순서(Startup Sequencing), 제어된 종료(Controlled Shutdown), 저전압 처리 및 전원 상태 모니터링이 중요하며, 갑작스러운 전원 차단은 저장장치 손상, 로그 중단 또는 임무 소프트웨어의 불확정 상태를 유발할 수 있다.

열 아키텍처(Thermal Architecture)는 지속 가능한 컴퓨팅 성능을 직접 결정한다. 명목상 추론 성능을 충족하는 젯슨 모듈이라도 높은 주변 온도나 지속적인 보행 조건에서 밀폐된 로봇 본체 내부에 설치되면 열 스로틀링(Thermal Throttling)이 발생할 수 있다. 따라서 히트 스프레더(Heat Spreader), 열전도 경로, 가능한 경우의 공기 흐름, 온도 텔레메트리(Telemetry), 워크로드 기반 전력 모드를 기계적 패키징과 함께 설계해야 한다.

소프트웨어 스택(Software Stack)은 하드웨어 종속적인 가속 기능과 응용 수준의 로봇 기능을 분리해야 한다. 리눅스(Linux)와 젯슨 소프트웨어 환경은 장치 드라이버와 GPU 서비스를 제공하고, CUDA 기반 라이브러리와 추론 런타임(Inference Runtime)은 인지 및 AI 모델을 가속한다. ROS 2는 카메라, 라이다, 위치추정, 지도작성, 경로계획, 진단 및 임무 기능을 명확한 인터페이스와 측정 가능한 타이밍 특성을 갖는 모듈형 노드로 구성할 수 있다.

AI 추론(AI Inference)은 단순 처리량(Throughput)이 아니라 종단간 지연시간(End-to-End Latency)을 중심으로 설계해야 한다. 불규칙한 지형을 이동하는 사족보행 로봇은 센서 관측을 제한된 시간 안에 유효한 운동 결정으로 변환해야 한다. 전처리, 추론, 후처리, 미들웨어 전송, 동기화 및 명령 전달 모두 응답 지연에 영향을 주므로 실제 센서와 보행 워크로드가 동시에 수행되는 조건에서 전체 파이프라인을 프로파일링(Profiling)해야 한다.

젯슨과 하위 제어 계층 사이의 통신은 명확한 제어 권한 경계(Control Authority Boundary)를 유지해야 한다. 상위 명령은 목표 몸체 속도, 궤적, 발 디딤 위치(Foothold Target), 보행 패턴 요청(Gait Request), 임무 상태 등으로 표현할 수 있다. 반면 관절 수준의 전류, 토크, 위치 및 보호 제어 루프는 결정론적 제어기가 담당해야 하며, 이를 통해 운영체제 스케줄링이나 GPU 부하 변동이 액추에이터 제어 안정성에 직접 영향을 미치는 것을 방지한다.

고장 격리(Fault Containment) 역시 중요하다. 젯슨 애플리케이션은 소프트웨어 결함, 메모리 고갈, 열 스로틀링, 손상된 센서 입력 또는 연산 과부하로 인해 실패할 수 있다. 이러한 고장이 기본적인 전기적 보호 기능이나 비상 정지(Emergency Stop) 기능까지 상실하게 해서는 안 된다. 독립적인 제어기는 액추에이터를 정의된 안전 상태로 전환할 수 있어야 하며, 워치독(Watchdog)과 상태 모니터링 메커니즘은 상위 컴퓨팅 노드의 장애를 감지해야 한다.

네트워크 아키텍처(Network Architecture)는 다수의 인지 센서와 분산 제어기에서 생성되는 대역폭을 고려해야 한다. 기가비트 이더넷(Gigabit Ethernet) 이상의 네트워크는 카메라, 라이다, 보조 컴퓨터 및 진단 장비를 연결하는 백본(Backbone)이 될 수 있으며, CAN FD 또는 EtherCAT은 결정성과 토폴로지 요구조건에 따라 제어 중심 서브시스템에 적용할 수 있다. 트래픽 우선순위화(Traffic Prioritization)를 통해 대용량 데이터나 로그 전송이 제어 관련 통신을 방해하지 않도록 해야 한다.

시간 동기화(Time Synchronization)는 젯슨 아키텍처와 로봇의 실제 물리 상태를 연결한다. 카메라 프레임, 라이다 스캔, IMU 샘플, 엔코더 상태 및 액추에이터 피드백은 정확한 센서 융합과 운동 상태 추정을 위해 일관된 시간 기준에 대응해야 한다. 하드웨어 타임스탬프(Hardware Timestamp)와 PTP 기반 동기화는 시간 불확실성을 줄일 수 있으며, 소프트웨어는 센서 취득 시각을 이후 처리 시각으로 대체하지 않고 타임스탬프 출처(Timestamp Provenance)를 보존해야 한다.

부팅 및 복구 동작(Boot and Recovery Behavior)도 시스템 아키텍처의 일부로 설계해야 한다. 전원이 공급되면 핵심 인터페이스, 시간 동기화, 센서 드라이버, 위치추정, AI 서비스 및 임무 소프트웨어가 제어된 의존성 순서에 따라 시작되어야 한다. 감독 로직은 단순히 초기화 중인 구성요소와 실제 고장난 구성요소를 구분해야 하며, 필요한 컴퓨팅 및 센싱 서비스가 검증된 운용 상태에 도달하기 전에는 보행이 시작되지 않도록 해야 한다.

진단 시스템(Diagnostics)은 CPU 사용률, GPU 사용률, 메모리 압력, 저장장치 상태, 온도, 전원 상태, 네트워크 통계, 추론 지연시간, 센서 데이터 최신성 및 프로세스 상태를 확인할 수 있어야 한다. 이러한 정보는 인지 실패와 통신 또는 열 문제를 구별하는 데 활용된다. 또한 과거 텔레메트리 데이터는 실험실에서 재현하기 어려운 간헐적 고장의 현장 디버깅(Field Debugging)과 예지정비(Predictive Maintenance)를 지원한다.

소프트웨어 업데이트(Software Update)는 업데이트 성공 여부에 복구 기능이 종속되지 않도록 설계해야 한다. 버전 관리된 AI 모델, 구성 관리(Configuration Control), 롤백(Rollback), 무결성 검증(Integrity Verification), 보호된 시스템 파티션을 통해 원격 배포 위험을 줄일 수 있다. 사족보행 로봇은 엔지니어와 멀리 떨어진 장소에서 운용될 수 있으므로 애플리케이션 소프트웨어, AI 모델 또는 네트워크 설정이 잘못되더라도 복구 경로를 유지해야 한다.

젯슨의 확장성(Scalability)은 기본형과 고성능 로봇 구성의 분리를 가능하게 한다. 저전력 플랫폼은 필수적인 인지와 내비게이션을 수행하고, 고성능 구성은 다중 신경망, 고밀도 3D 인지(Dense 3D Perception), VLM 기반 상황 해석 또는 고급 검사 분석을 지원할 수 있다. 이러한 구성 사이에 공통 인터페이스를 유지하면 소프트웨어 파편화(Software Fragmentation)를 줄이고 컴퓨팅 하드웨어가 발전하더라도 이식성(Portability)을 유지할 수 있다.

고급 피지컬 AI(Physical AI) 환경에서 젯슨은 의미론적 지능(Semantic Intelligence)과 물리적 실행(Physical Execution)을 연결하는 가교 역할을 한다. 인지 모델은 환경을 해석하고, 학습된 표현(Learned Representation)은 상황적 이해를 제공하며, 계획 구성요소는 이러한 이해를 보행 제어와 호환되는 목표로 변환한다. 그러나 확률적 AI 출력과 안전성, 안정성, 액추에이터 한계 및 시스템 상태가 요구하는 결정론적 제약조건 사이의 구분은 항상 유지되어야 한다.

따라서 잘 설계된 젯슨 아키텍처(Jetson Architecture)는 단순히 강력한 GPU 모듈을 로봇에 장착하는 것으로 정의되지 않는다. AI 가속, 센서 인터페이스, 결정론적 제어기 연계, 네트워킹, 시간 동기화, 전원 관리, 열 설계, 진단 및 고장 격리를 통합한 컴퓨팅 서브시스템이어야 한다. 이러한 구조는 이후 다루는 엣지 GPU 서버(Edge GPU Server), 실시간 제어기(Real-Time Controller), AI 가속(AI Acceleration), PTP 시간 동기화(PTP Time Synchronization), 컴퓨팅 이중화(Compute Redundancy)를 위한 기반을 제공한다.

##  

## 08.02. Edge GPU Server

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

An Edge GPU Server extends the quadruped robot's computing architecture beyond the capabilities of a single embedded Jetson node. It provides a higher-performance computing tier for workloads that require greater GPU memory, parallel processing capacity, or sustained inference throughput. The server complements onboard real-time control and embedded AI rather than replacing them, creating a hierarchical computing architecture optimized for demanding Physical AI workloads.

The server can execute computationally intensive perception, multimodal inference, dense three-dimensional reconstruction, large neural networks, Vision-Language Models, and advanced planning algorithms. By moving selected workloads away from the embedded computer, the robot preserves onboard resources for latency-sensitive navigation and essential autonomy. Workload placement should therefore reflect latency, bandwidth, reliability, power consumption, and mission-criticality rather than GPU performance alone.

A typical Edge GPU Server combines one or more high-performance GPUs with a multicore CPU, large system memory, high-speed NVMe storage, and high-bandwidth networking. Unlike a cloud server, it is deployed physically close to the robot or robot fleet. This proximity reduces communication latency and allows large sensor datasets to remain within the operational site while still providing substantially greater computing capability than an embedded platform.

The relationship between Jetson and the Edge GPU Server should be organized as a cooperative computing hierarchy. Jetson maintains onboard perception, localization, navigation, mission continuity, and communication with the real-time controller, while the server processes computationally expensive or less latency-critical functions. If server connectivity is lost, the robot should retain a defined minimum autonomous capability instead of becoming completely dependent on external computation.

Workload partitioning is one of the most important architectural decisions. Functions requiring immediate physical response should remain onboard, whereas workloads involving large models, multiple sensor streams, global optimization, semantic reasoning, or fleet-wide information can be assigned to the server. A practical architecture may dynamically migrate or replicate selected inference services according to network quality, GPU utilization, mission priority, and robot operating condition.

Multi-GPU configurations provide additional scalability when a single accelerator cannot satisfy model size or throughput requirements. Independent GPUs may execute different perception and reasoning services, while larger models can use model parallelism or other distributed execution techniques. The architecture must consider GPU memory capacity, inter-GPU data movement, CPU-to-GPU transfer overhead, scheduling contention, and synchronization rather than assuming that additional GPUs automatically produce proportional performance gains.

GPU memory is especially important for advanced Physical AI. High-resolution camera inputs, LiDAR tensors, temporal features, occupancy representations, world models, and large foundation models can consume substantial memory before inference even begins. Larger GPU memory enables larger models, larger batches, longer temporal contexts, and simultaneous execution of multiple AI services, but memory bandwidth and data-transfer efficiency remain equally important to sustained performance.

The CPU subsystem coordinates networking, storage, process scheduling, preprocessing, middleware, and system management. Sufficient CPU resources are required to keep high-performance GPUs continuously supplied with useful work. A poorly balanced server can leave expensive accelerators underutilized because decoding, sensor conversion, middleware transport, or storage access becomes the actual bottleneck. System design must therefore consider the complete data path rather than GPU specifications in isolation.

High-speed networking forms the connection between the quadruped and the Edge GPU Server. Gigabit Ethernet may support moderate workloads, while multi-gigabit or higher-speed Ethernet becomes important when multiple high-resolution cameras, LiDAR streams, feature tensors, or several robots share the same infrastructure. Network design should account for aggregate bandwidth, packet loss, congestion, deterministic traffic requirements, and separation between control-related and bulk data flows.

Time synchronization remains essential when processing is distributed across different computers. Sensor data generated onboard must retain its original acquisition timestamp when transmitted to the Edge GPU Server. PTP-based synchronization and hardware timestamping can establish a common temporal reference among robots, Jetson nodes, sensors, and servers. Accurate timing enables remote sensor fusion and prevents network delay from being incorrectly interpreted as physical-world timing.

The server storage subsystem supports AI models, maps, recorded datasets, logs, cached sensor data, software containers, and diagnostic information. NVMe storage provides the throughput required for high-rate recording and rapid model loading. Storage architecture should separate temporary data from mission-critical software and configuration, while retention policies prevent uncontrolled sensor recording from exhausting available capacity during long-duration operation.

Software deployment should provide a consistent execution environment between development systems, Jetson devices, and Edge GPU Servers. Containerized services can isolate dependencies and simplify version control, deployment, rollback, and hardware-specific optimization. ROS 2, DDS, gRPC, or similar communication mechanisms may connect distributed services, but interface definitions should remain stable even when a model moves between onboard and server-side execution.

AI acceleration on the server should be evaluated using end-to-end mission performance rather than theoretical GPU throughput. Metrics should include inference latency, P50/P95/P99 response time, worst-case behavior, memory utilization, data-transfer overhead, and performance under concurrent workloads. A server that performs well for one isolated neural network may behave very differently when perception, VLM reasoning, mapping, logging, and multiple robots operate simultaneously.

Thermal and power requirements differ substantially from embedded Jetson computing. High-performance GPUs can demand hundreds of watts each and generate concentrated thermal loads. The Edge GPU Server therefore requires appropriate power conversion, cooling, airflow, environmental protection, and power monitoring. For mobile or field-deployed systems, the energy required by the server may determine whether it is mounted on a carrier platform, installed in a nearby cabinet, or operated from fixed infrastructure.

Fault isolation should prevent Edge GPU Server failures from propagating into locomotion control. Server crashes, network interruptions, GPU errors, thermal shutdowns, or overloaded inference services must be detectable by the onboard system. Jetson can monitor service heartbeat, communication latency, and response freshness, rejecting outdated results and switching to local fallback functions when external computation becomes unavailable or unreliable.

Security becomes increasingly important because the server creates a high-bandwidth digital connection to the robot. Authentication, encrypted communication, access control, software integrity verification, and controlled service exposure help prevent unauthorized commands or model manipulation. Network segmentation can further separate robot control, sensor transport, maintenance access, and external enterprise networks, reducing the consequences of failures or cybersecurity incidents.

For multiple quadrupeds, the Edge GPU Server can evolve from a companion computer into shared fleet intelligence infrastructure. Several robots may contribute sensor observations while the server performs global mapping, semantic scene understanding, centralized analytics, model inference, or mission coordination. Resource scheduling then becomes essential so that one robot or computationally expensive model cannot consume all GPU capacity and degrade services required by the rest of the fleet.

An Edge GPU Server also creates an intermediate layer between individual robots and cloud AI. Sensitive or high-volume sensor data can be processed locally, while only selected results, compressed representations, reports, or model updates are exchanged with cloud systems. This reduces external bandwidth requirements and enables operation in environments where internet connectivity is limited, intermittent, expensive, or restricted by operational security requirements.

In advanced Physical AI architectures, the server can host larger semantic and reasoning models that are impractical on the robot. Jetson handles immediate embodied interaction, the real-time controller maintains deterministic physical execution, and the Edge GPU Server provides deeper perception, world representation, reasoning, and fleet-level intelligence. This separation aligns computational responsibility with the timing and resource characteristics of each layer.

The resulting architecture should therefore be viewed as a distributed computing continuum rather than a simple external GPU expansion. Real-time controllers protect deterministic motion, Jetson maintains local autonomy, and the Edge GPU Server supplies scalable AI computation close to the operational environment. Together, these layers provide the foundation for subsequent real-time control, AI acceleration, PTP synchronization, and compute redundancy within the quadruped computing architecture.

엣지 GPU 서버(Edge GPU Server)는 단일 임베디드 젯슨 노드(Embedded Jetson Node)의 성능 한계를 넘어 사족보행 로봇의 컴퓨팅 아키텍처를 확장한다. 더 큰 GPU 메모리, 병렬 처리 능력 또는 지속적인 추론 처리량이 필요한 워크로드를 위해 고성능 컴퓨팅 계층을 제공한다. 서버는 온보드 실시간 제어(Onboard Real-Time Control)와 임베디드 AI(Embedded AI)를 대체하지 않고 보완하며, 고부하 피지컬 AI(Physical AI) 워크로드에 최적화된 계층형 컴퓨팅 아키텍처를 구성한다.

서버는 연산 집약적인 인지(Perception), 멀티모달 추론(Multimodal Inference), 고밀도 3차원 재구성(Dense Three-Dimensional Reconstruction), 대규모 신경망, 비전-언어 모델(Vision-Language Model, VLM), 고급 경로 계획 알고리즘을 실행할 수 있다. 일부 워크로드를 임베디드 컴퓨터에서 분리함으로써 로봇은 지연시간에 민감한 내비게이션과 핵심 자율 기능을 위한 온보드 자원을 확보할 수 있다. 따라서 워크로드 배치는 GPU 성능뿐만 아니라 지연시간, 대역폭, 신뢰성, 전력 소비 및 임무 중요도를 고려해야 한다.

일반적인 엣지 GPU 서버(Edge GPU Server)는 하나 이상의 고성능 GPU와 멀티코어 CPU(Multicore CPU), 대용량 시스템 메모리, 고속 NVMe 저장장치 및 고대역폭 네트워크를 결합한다. 클라우드 서버(Cloud Server)와 달리 로봇 또는 로봇 플릿(Robot Fleet)과 물리적으로 가까운 위치에 배치된다. 이러한 근접성은 통신 지연을 줄이고 대규모 센서 데이터를 운용 현장 내부에 유지하면서도 임베디드 플랫폼보다 훨씬 높은 컴퓨팅 성능을 제공한다.

젯슨(Jetson)과 엣지 GPU 서버의 관계는 협력형 컴퓨팅 계층(Cooperative Computing Hierarchy)으로 구성해야 한다. 젯슨은 온보드 인지, 위치추정, 내비게이션, 임무 연속성 및 실시간 제어기와의 통신을 유지하고, 서버는 연산량이 크거나 상대적으로 지연시간 민감도가 낮은 기능을 처리한다. 서버 연결이 끊어지더라도 로봇이 외부 연산에 완전히 의존하지 않고 정의된 최소 자율 기능(Minimum Autonomous Capability)을 유지할 수 있어야 한다.

워크로드 분할(Workload Partitioning)은 가장 중요한 아키텍처 결정 중 하나이다. 즉각적인 물리적 반응이 필요한 기능은 온보드에 유지하고, 대규모 모델, 다중 센서 스트림, 전역 최적화(Global Optimization), 의미론적 추론(Semantic Reasoning), 플릿 전체 정보를 처리하는 워크로드는 서버에 할당할 수 있다. 실제 아키텍처에서는 네트워크 품질, GPU 사용률, 임무 우선순위 및 로봇 운용 상태에 따라 일부 추론 서비스를 동적으로 이동하거나 복제할 수 있다.

다중 GPU 구성(Multi-GPU Configuration)은 하나의 가속기만으로 모델 크기나 처리량 요구조건을 충족할 수 없을 때 추가적인 확장성을 제공한다. 각각의 GPU가 서로 다른 인지 및 추론 서비스를 실행할 수 있으며, 대규모 모델은 모델 병렬화(Model Parallelism) 또는 다른 분산 실행 기법을 사용할 수 있다. 추가 GPU가 자동으로 비례적인 성능 향상을 제공한다고 가정해서는 안 되며, GPU 메모리 용량, GPU 간 데이터 이동, CPU-GPU 전송 오버헤드, 스케줄링 경합 및 동기화를 함께 고려해야 한다.

GPU 메모리는 고급 피지컬 AI(Physical AI)에서 특히 중요하다. 고해상도 카메라 입력, 라이다 텐서(LiDAR Tensor), 시간 특징(Temporal Feature), 점유 표현(Occupancy Representation), 월드 모델(World Model), 대규모 파운데이션 모델(Foundation Model)은 추론이 시작되기 전부터 상당한 메모리를 소비할 수 있다. 더 큰 GPU 메모리는 더 큰 모델과 배치, 더 긴 시간 문맥(Temporal Context), 여러 AI 서비스의 동시 실행을 가능하게 하지만, 지속적인 성능을 위해서는 메모리 대역폭과 데이터 전송 효율 역시 중요하다.

CPU 서브시스템(CPU Subsystem)은 네트워킹, 저장장치, 프로세스 스케줄링, 전처리, 미들웨어 및 시스템 관리를 조정한다. 고성능 GPU에 지속적으로 유효한 작업을 공급하려면 충분한 CPU 자원이 필요하다. 균형이 맞지 않는 서버에서는 디코딩, 센서 변환, 미들웨어 전송 또는 저장장치 접근이 실제 병목이 되어 고가의 가속기가 충분히 활용되지 못할 수 있다. 따라서 시스템은 GPU 사양만이 아니라 전체 데이터 경로를 고려하여 설계해야 한다.

고속 네트워크(High-Speed Networking)는 사족보행 로봇과 엣지 GPU 서버를 연결한다. 기가비트 이더넷(Gigabit Ethernet)은 중간 수준의 워크로드를 지원할 수 있지만, 다수의 고해상도 카메라, 라이다 스트림, 특징 텐서 또는 여러 로봇이 동일한 인프라를 공유하는 경우 멀티기가비트 이상의 이더넷이 중요해진다. 네트워크 설계에서는 전체 대역폭, 패킷 손실, 혼잡, 결정론적 트래픽 요구조건 및 제어 관련 데이터와 대용량 데이터 흐름의 분리를 고려해야 한다.

서로 다른 컴퓨터에 처리가 분산되는 경우에도 시간 동기화(Time Synchronization)는 필수적이다. 온보드에서 생성된 센서 데이터는 엣지 GPU 서버로 전송될 때 원래의 취득 타임스탬프(Acquisition Timestamp)를 유지해야 한다. PTP 기반 동기화와 하드웨어 타임스탬핑(Hardware Timestamping)은 로봇, 젯슨 노드, 센서 및 서버 사이에 공통 시간 기준을 형성할 수 있다. 정확한 시간 정보는 원격 센서 융합을 가능하게 하고 네트워크 지연이 실제 물리 세계의 시간으로 잘못 해석되는 것을 방지한다.

서버 저장장치 서브시스템(Storage Subsystem)은 AI 모델, 지도, 기록 데이터셋, 로그, 캐시된 센서 데이터, 소프트웨어 컨테이너 및 진단 정보를 저장한다. NVMe 저장장치는 고속 데이터 기록과 빠른 모델 로딩에 필요한 처리량을 제공한다. 저장장치 아키텍처에서는 임시 데이터와 임무 핵심 소프트웨어 및 설정을 분리해야 하며, 보존 정책(Retention Policy)을 통해 장시간 운용 중 센서 기록이 저장 공간을 무제한으로 소모하는 것을 방지해야 한다.

소프트웨어 배포(Software Deployment)는 개발 시스템, 젯슨 장치 및 엣지 GPU 서버 사이에서 일관된 실행 환경을 제공해야 한다. 컨테이너화된 서비스(Containerized Service)는 의존성을 격리하고 버전 관리, 배포, 롤백 및 하드웨어별 최적화를 단순화할 수 있다. ROS 2, DDS, gRPC 또는 유사한 통신 메커니즘을 이용하여 분산 서비스를 연결할 수 있지만, 모델이 온보드와 서버 사이에서 이동하더라도 인터페이스 정의는 안정적으로 유지되어야 한다.

서버의 AI 가속(AI Acceleration)은 이론적인 GPU 처리량이 아니라 종단간 임무 성능(End-to-End Mission Performance)을 기준으로 평가해야 한다. 평가 지표에는 추론 지연시간, P50/P95/P99 응답시간, 최악조건 동작(Worst-Case Behavior), 메모리 사용률, 데이터 전송 오버헤드 및 동시 워크로드 환경에서의 성능이 포함되어야 한다. 하나의 신경망만 실행할 때 우수한 서버라도 인지, VLM 추론, 지도작성, 로깅 및 다수 로봇이 동시에 동작하면 전혀 다른 성능 특성을 보일 수 있다.

열 및 전력 요구조건(Thermal and Power Requirements)은 임베디드 젯슨 컴퓨팅과 크게 다르다. 고성능 GPU는 각각 수백 와트의 전력을 요구하고 집중적인 열부하를 발생시킬 수 있다. 따라서 엣지 GPU 서버에는 적절한 전력 변환, 냉각, 공기 흐름, 환경 보호 및 전력 모니터링이 필요하다. 이동형 또는 현장 배치 시스템에서는 서버의 에너지 요구량에 따라 운반 플랫폼(Carrier Platform)에 탑재하거나 인근 캐비닛 또는 고정 인프라에 설치할 수 있다.

고장 격리(Fault Isolation)는 엣지 GPU 서버의 장애가 보행 제어로 전파되지 않도록 해야 한다. 서버 충돌, 네트워크 단절, GPU 오류, 열적 종료 또는 추론 서비스 과부하는 온보드 시스템에서 감지할 수 있어야 한다. 젯슨은 서비스 하트비트(Service Heartbeat), 통신 지연시간 및 응답 최신성(Response Freshness)을 감시하고, 오래된 결과를 거부하며 외부 연산을 사용할 수 없거나 신뢰할 수 없을 때 로컬 대체 기능(Local Fallback Function)으로 전환할 수 있다.

서버가 로봇과 고대역폭 디지털 연결을 형성하므로 보안(Security)의 중요성도 증가한다. 인증(Authentication), 암호화 통신, 접근 제어, 소프트웨어 무결성 검증 및 서비스 노출 제어를 통해 비인가 명령이나 모델 변조 위험을 줄일 수 있다. 네트워크 분할(Network Segmentation)을 통해 로봇 제어, 센서 전송, 유지보수 접근 및 외부 기업 네트워크를 분리하면 장애나 사이버보안 사고의 영향을 제한할 수 있다.

여러 대의 사족보행 로봇을 운용할 경우 엣지 GPU 서버는 보조 컴퓨터(Companion Computer)를 넘어 공유 플릿 지능 인프라(Shared Fleet Intelligence Infrastructure)로 발전할 수 있다. 여러 로봇이 센서 관측 정보를 제공하고 서버가 전역 지도작성, 의미론적 장면 이해, 중앙집중형 분석, 모델 추론 또는 임무 조정을 수행할 수 있다. 이 경우 하나의 로봇이나 연산량이 큰 모델이 모든 GPU 자원을 점유하여 다른 로봇의 서비스를 저하시키지 않도록 자원 스케줄링(Resource Scheduling)이 필수적이다.

엣지 GPU 서버는 개별 로봇과 클라우드 AI(Cloud AI) 사이의 중간 계층도 형성한다. 민감하거나 대용량인 센서 데이터를 현장에서 처리하고 선택된 결과, 압축 표현(Compressed Representation), 보고서 또는 모델 업데이트만 클라우드 시스템과 교환할 수 있다. 이를 통해 외부 네트워크 대역폭 요구량을 줄이고 인터넷 연결이 제한적이거나 불안정하고 비용이 높거나 운용 보안 요구사항에 의해 제한되는 환경에서도 동작할 수 있다.

고급 피지컬 AI 아키텍처(Physical AI Architecture)에서 서버는 로봇 자체에서 실행하기 어려운 대규모 의미론 및 추론 모델을 호스팅할 수 있다. 젯슨은 즉각적인 체화 상호작용(Embodied Interaction)을 담당하고, 실시간 제어기는 결정론적 물리 실행(Deterministic Physical Execution)을 유지하며, 엣지 GPU 서버는 더욱 심층적인 인지, 월드 표현(World Representation), 추론 및 플릿 수준 지능(Fleet-Level Intelligence)을 제공한다. 이러한 분리는 각 계층의 시간 및 자원 특성에 맞게 연산 책임을 배치한다.

따라서 최종 아키텍처는 단순한 외부 GPU 확장이 아니라 분산 컴퓨팅 연속체(Distributed Computing Continuum)로 이해해야 한다. 실시간 제어기(Real-Time Controller)는 결정론적 운동을 보호하고, 젯슨(Jetson)은 로컬 자율성(Local Autonomy)을 유지하며, 엣지 GPU 서버(Edge GPU Server)는 운용 환경 가까이에서 확장 가능한 AI 연산을 제공한다. 이러한 계층들은 함께 사족보행 로봇 컴퓨팅 아키텍처의 후속 주제인 실시간 제어(Real-Time Control), AI 가속(AI Acceleration), PTP 시간 동기화(PTP Time Synchronization), 컴퓨팅 이중화(Compute Redundancy)의 기반을 형성한다.

##  

## 08.03. Real Time Controller

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

A Real-Time Controller forms the deterministic execution layer between high-level computing and the physical joints of a quadruped robot. While Jetson or an Edge GPU Server performs perception, localization, planning, and AI inference, the real-time controller executes timing-critical motion functions with predictable latency. This separation prevents variable AI workloads and operating-system scheduling from directly disturbing robot stability and actuator control.

Quadruped locomotion requires coordinated control of multiple joints operating at high update rates. Hip, upper-leg, knee, and lower-leg actuators must respond consistently to commanded positions, velocities, or torques while the robot experiences rapidly changing ground forces. The controller therefore maintains periodic execution cycles with bounded jitter, ensuring that sensing, control computation, communication, and actuator commands occur within defined timing constraints.

The real-time controller typically receives higher-level motion objectives from the Jetson computing layer. These objectives may include desired body velocity, orientation, trajectory, gait mode, foothold targets, or locomotion state. Instead of directly commanding motor power from AI software, the controller converts these objectives into deterministic joint-level references while enforcing physical constraints such as position, velocity, torque, current, and thermal limits.

At the lower interface, the controller communicates with distributed joint modules containing motor drivers, encoders, and potentially torque or temperature sensors. Joint position and velocity feedback provide the fundamental state required for closed-loop control, while torque and current measurements help estimate mechanical loading. The controller continuously combines these signals to regulate actuator behavior and detect abnormal conditions before they develop into larger system failures.

Control-loop timing must be engineered from the complete signal path rather than from processor speed alone. Sensor sampling, bus transmission, task scheduling, control calculation, command transmission, and motor-driver response all contribute to effective loop latency. A fast processor cannot compensate for unpredictable communication delays or excessive jitter, so timing analysis must consider worst-case execution and communication behavior throughout the entire control chain.

A real-time operating system or equivalent deterministic execution environment can organize periodic control tasks according to explicit priorities and deadlines. High-priority actuator and safety functions should remain isolated from lower-priority logging, diagnostics, or configuration services. Memory allocation, blocking operations, background processes, and uncontrolled communication should also be minimized in critical paths because they can introduce timing variations that degrade locomotion performance.

CAN FD can provide robust communication for distributed joint and auxiliary controllers where bandwidth requirements are moderate and fault handling is important. EtherCAT can support higher-rate synchronized control when tighter timing and coordinated multi-axis operation are required. The appropriate network depends on joint count, update frequency, payload size, topology, synchronization requirements, wiring constraints, and the required level of determinism.

The controller must maintain an accurate representation of robot state. Encoder measurements describe joint configuration, while IMU information contributes body orientation and angular motion. Foot force sensors or torque estimates indicate contact with the ground. Combining these signals allows the control system to determine whether each leg is supporting the body, transitioning through swing, encountering an obstacle, or experiencing unexpected mechanical loading.

Gait execution requires precise coordination among all four legs rather than independent joint control. Walking, trotting, crawling, climbing, and recovery behaviors impose different timing relationships between stance and swing phases. The real-time controller translates the selected gait strategy into synchronized joint references and maintains phase consistency even when terrain contact or actuator response differs from the nominal model.

Safety constraints must remain active independently of high-level AI decisions. The controller should reject commands that exceed allowable joint position, speed, torque, or current ranges and should detect stale, missing, or inconsistent commands. When communication with the Jetson node is interrupted, the controller must transition according to a predefined policy rather than continuing indefinitely with the last received motion command.

Emergency-stop handling requires an explicit relationship between logical control and electrical power architecture. Software can request a controlled stop or torque reduction, but critical emergency functions should not depend exclusively on the real-time software remaining operational. Independent safety paths can disable actuator enable signals or command the power architecture toward a defined safe state when hazardous conditions are detected.

Watchdog mechanisms provide another layer of fault containment. A hardware or independent supervisory watchdog can verify that critical controller tasks continue executing within expected timing limits. If the controller stalls, misses repeated deadlines, or produces invalid health signals, the watchdog can initiate a controlled reset, inhibit actuator operation, or trigger another defined response according to the system safety architecture.

Fault handling should distinguish between failures that require immediate shutdown and those that permit degraded operation. A temporary sensor anomaly may allow reduced-speed locomotion, whereas loss of essential joint feedback can require stopping the affected leg or the entire robot. This distinction supports fail-safe and potentially fail-operational strategies while preventing minor faults from unnecessarily causing complete mission termination.

Communication with Jetson should use clearly defined command and state interfaces. Jetson provides high-level objectives, while the controller returns joint states, body states, actuator temperatures, fault information, execution status, and timing health. Sequence counters, timestamps, validity indicators, and timeout mechanisms help determine whether received information is current and trustworthy rather than merely present on the communication interface.

Time synchronization becomes particularly important when control information is combined with camera, LiDAR, IMU, and other sensor observations. The real-time controller may operate with a local high-resolution clock while participating in a broader synchronized time domain. PTP or related synchronization mechanisms can align distributed timestamps, allowing high-level computing to associate physical joint motion with sensor observations accurately.

Diagnostics should monitor both functional behavior and timing behavior. CPU utilization alone does not indicate whether real-time performance is healthy. Useful information includes control-cycle duration, maximum execution time, deadline misses, communication latency, packet errors, actuator response, sensor freshness, watchdog status, and synchronization quality. Recording these metrics helps identify intermittent problems that appear only during demanding locomotion or AI workloads.

The controller should also support calibration and commissioning procedures without compromising operational safety. Joint zero positions, encoder offsets, torque-sensor calibration, motor direction, current limits, and mechanical range parameters must be stored and validated. Configuration data should be version controlled and protected against accidental modification because incorrect calibration can produce physically dangerous motion even when the control algorithms themselves operate correctly.

Startup sequencing ensures that actuator control begins only after required components have reached valid states. Communication buses, sensors, joint modules, calibration parameters, safety inputs, and high-level command interfaces should be checked before torque is enabled. Shutdown should similarly transition the robot toward a mechanically stable condition before actuator power is removed whenever operating conditions allow a controlled sequence.

The boundary between AI and deterministic control becomes increasingly important as Physical AI capabilities expand. Learned models may generate trajectories, footholds, gait selections, or semantic motion objectives, but their outputs should pass through deterministic validation and constraint enforcement before reaching actuators. This architecture allows advanced intelligence to influence behavior without granting probabilistic AI models unrestricted authority over safety-critical motor execution.

A robust Real-Time Controller therefore acts as the trusted physical execution layer of the quadruped computing architecture. It connects Jetson-based intelligence with distributed actuators through deterministic scheduling, synchronized communication, closed-loop control, diagnostics, watchdogs, and safety constraints. Together with the Jetson Architecture and Edge GPU Server, it establishes a hierarchical foundation for subsequent AI acceleration, PTP time synchronization, and compute redundancy.

실시간 제어기(Real-Time Controller)는 사족보행 로봇에서 상위 컴퓨팅(High-Level Computing)과 물리적 관절(Physical Joint) 사이를 연결하는 결정론적 실행 계층(Deterministic Execution Layer)을 구성한다. 젯슨(Jetson)이나 엣지 GPU 서버(Edge GPU Server)가 인지, 위치추정, 경로 계획 및 AI 추론을 수행하는 동안 실시간 제어기는 예측 가능한 지연시간으로 시간 임계적인 운동 기능을 실행한다. 이러한 분리는 가변적인 AI 워크로드와 운영체제 스케줄링이 로봇의 안정성과 액추에이터 제어에 직접적인 영향을 주는 것을 방지한다.

사족보행 로봇의 보행(Locomotion)은 높은 갱신 주기로 동작하는 여러 관절의 협조 제어를 필요로 한다. 엉덩이(Hip), 상부 다리(Upper Leg), 무릎(Knee), 하부 다리(Lower Leg)의 액추에이터는 로봇에 작용하는 지면 반력이 빠르게 변화하는 상황에서도 명령된 위치, 속도 또는 토크에 일관되게 반응해야 한다. 따라서 제어기는 제한된 지터(Bounded Jitter)를 갖는 주기적 실행 사이클을 유지하여 센싱, 제어 연산, 통신 및 액추에이터 명령이 정의된 시간 제약 내에서 수행되도록 한다.

실시간 제어기는 일반적으로 젯슨 컴퓨팅 계층(Jetson Computing Layer)으로부터 상위 수준의 운동 목표(Motion Objective)를 수신한다. 이러한 목표에는 원하는 몸체 속도, 자세, 궤적, 보행 모드(Gait Mode), 발 디딤 목표(Foothold Target), 보행 상태 등이 포함될 수 있다. AI 소프트웨어가 모터 출력을 직접 명령하는 대신 제어기가 이러한 목표를 결정론적인 관절 수준 기준값으로 변환하면서 위치, 속도, 토크, 전류 및 열적 한계와 같은 물리적 제약조건을 적용한다.

하위 인터페이스에서 제어기는 모터 드라이버(Motor Driver), 엔코더(Encoder), 그리고 필요에 따라 토크 또는 온도 센서를 포함하는 분산 관절 모듈(Distributed Joint Module)과 통신한다. 관절 위치와 속도 피드백은 폐루프 제어(Closed-Loop Control)에 필요한 기본 상태를 제공하며, 토크와 전류 측정값은 기계적 부하를 추정하는 데 사용된다. 제어기는 이러한 신호를 지속적으로 결합하여 액추에이터 동작을 조절하고 이상 상태가 더 큰 시스템 고장으로 발전하기 전에 감지한다.

제어 루프 타이밍(Control-Loop Timing)은 프로세서 속도만이 아니라 전체 신호 경로를 기준으로 설계해야 한다. 센서 샘플링, 버스 전송, 태스크 스케줄링, 제어 연산, 명령 전송 및 모터 드라이버 응답이 모두 실질적인 루프 지연시간에 영향을 준다. 빠른 프로세서라도 예측 불가능한 통신 지연이나 과도한 지터를 보상할 수 없으므로 전체 제어 체인에서 최악조건 실행시간(Worst-Case Execution Time)과 통신 동작을 고려해야 한다.

실시간 운영체제(Real-Time Operating System) 또는 이에 상응하는 결정론적 실행 환경은 명시적인 우선순위와 마감시간(Deadline)에 따라 주기적인 제어 태스크를 구성할 수 있다. 우선순위가 높은 액추에이터 및 안전 기능은 우선순위가 낮은 로깅, 진단 또는 설정 서비스와 격리해야 한다. 메모리 할당, 블로킹 연산(Blocking Operation), 백그라운드 프로세스 및 통제되지 않은 통신도 임계 경로에서 최소화하여 보행 성능을 저하시키는 타이밍 변동을 줄여야 한다.

CAN FD는 대역폭 요구량이 중간 수준이고 고장 처리가 중요한 분산 관절 및 보조 제어기에 견고한 통신을 제공할 수 있다. EtherCAT은 더욱 엄격한 타이밍과 다축 동기 제어(Coordinated Multi-Axis Control)가 필요한 경우 고속 동기 제어를 지원할 수 있다. 적절한 네트워크는 관절 수, 갱신 주기, 페이로드 크기, 토폴로지, 동기화 요구사항, 배선 제약조건 및 필요한 결정성 수준에 따라 선택해야 한다.

제어기는 정확한 로봇 상태 표현(Robot State Representation)을 유지해야 한다. 엔코더 측정값은 관절 구성을 나타내며 IMU 정보는 몸체 자세와 각운동을 제공한다. 발 힘 센서(Foot Force Sensor) 또는 토크 추정값은 지면과의 접촉 상태를 나타낸다. 이러한 신호를 결합하면 각 다리가 몸체를 지지하고 있는지, 스윙 단계(Swing Phase)를 수행하고 있는지, 장애물과 접촉했는지 또는 예상하지 못한 기계적 부하를 받고 있는지를 판단할 수 있다.

보행 실행(Gait Execution)은 각각의 관절을 독립적으로 제어하는 것이 아니라 네 다리 전체의 정밀한 협조를 필요로 한다. 걷기, 트로트(Trot), 저속 보행(Crawl), 등반 및 자세 복구 동작은 입각 단계(Stance Phase)와 스윙 단계 사이에 서로 다른 시간 관계를 요구한다. 실시간 제어기는 선택된 보행 전략을 동기화된 관절 기준값으로 변환하고 지면 접촉이나 액추에이터 응답이 명목 모델과 달라지더라도 위상 일관성(Phase Consistency)을 유지한다.

안전 제약조건(Safety Constraint)은 상위 AI의 판단과 독립적으로 항상 활성화되어야 한다. 제어기는 허용 가능한 관절 위치, 속도, 토크 또는 전류 범위를 초과하는 명령을 거부하고 오래되거나 누락되었거나 일관성이 없는 명령을 감지해야 한다. 젯슨 노드와의 통신이 중단되는 경우 마지막으로 수신한 운동 명령을 무기한 실행하는 대신 사전에 정의된 정책에 따라 안전한 상태로 전환해야 한다.

비상 정지(Emergency Stop) 처리는 논리적 제어와 전기적 전원 아키텍처 사이의 명확한 관계를 필요로 한다. 소프트웨어는 제어된 정지 또는 토크 감소를 요청할 수 있지만 핵심적인 비상 기능이 실시간 소프트웨어의 정상 동작에만 의존해서는 안 된다. 독립적인 안전 경로(Independent Safety Path)를 통해 위험 상태가 감지되면 액추에이터 활성화 신호를 차단하거나 전원 아키텍처가 정의된 안전 상태로 전환되도록 할 수 있다.

워치독 메커니즘(Watchdog Mechanism)은 추가적인 고장 격리 계층을 제공한다. 하드웨어 또는 독립적인 감독 워치독(Supervisory Watchdog)은 핵심 제어 태스크가 예상된 타이밍 범위 내에서 계속 실행되는지 확인할 수 있다. 제어기가 정지하거나 반복적으로 마감시간을 초과하거나 유효하지 않은 상태 신호를 생성하면 워치독은 시스템 안전 아키텍처에 따라 제어된 재시작, 액추에이터 동작 억제 또는 다른 정의된 대응을 수행할 수 있다.

고장 처리(Fault Handling)는 즉각적인 시스템 정지가 필요한 고장과 성능 저하 상태에서 운용을 지속할 수 있는 고장을 구분해야 한다. 일시적인 센서 이상은 속도를 낮춘 보행을 허용할 수 있지만 핵심적인 관절 피드백이 상실되면 해당 다리 또는 전체 로봇을 정지해야 할 수 있다. 이러한 구분은 페일세이프(Fail-Safe) 및 경우에 따라 페일오퍼레이셔널(Fail-Operational) 전략을 지원하면서 경미한 고장이 불필요하게 전체 임무 중단으로 이어지는 것을 방지한다.

젯슨과의 통신은 명확하게 정의된 명령 및 상태 인터페이스(Command and State Interface)를 사용해야 한다. 젯슨은 상위 수준의 목표를 제공하고 제어기는 관절 상태, 몸체 상태, 액추에이터 온도, 고장 정보, 실행 상태 및 타이밍 상태를 반환한다. 시퀀스 카운터(Sequence Counter), 타임스탬프, 유효성 표시(Validity Indicator) 및 타임아웃 메커니즘을 이용하면 수신된 정보가 단순히 통신 인터페이스에 존재하는 것이 아니라 최신이며 신뢰할 수 있는지를 판단할 수 있다.

제어 정보가 카메라, 라이다(LiDAR), IMU 및 다른 센서 관측 정보와 결합될 때 시간 동기화(Time Synchronization)는 특히 중요해진다. 실시간 제어기는 로컬 고해상도 클록(Local High-Resolution Clock)을 사용하면서 더 넓은 동기화 시간 도메인(Synchronized Time Domain)에 참여할 수 있다. PTP 또는 관련 동기화 메커니즘을 이용하여 분산된 타임스탬프를 정렬하면 상위 컴퓨팅 계층에서 물리적인 관절 운동과 센서 관측을 정확하게 연계할 수 있다.

진단(Diagnostics)은 기능적 동작뿐만 아니라 타이밍 동작도 모니터링해야 한다. CPU 사용률만으로는 실시간 성능이 정상적인지 판단할 수 없다. 유용한 정보에는 제어 사이클 시간, 최대 실행시간, 마감시간 초과(Deadline Miss), 통신 지연, 패킷 오류, 액추에이터 응답, 센서 데이터 최신성, 워치독 상태 및 동기화 품질 등이 포함된다. 이러한 지표를 기록하면 고부하 보행이나 AI 워크로드 중에만 발생하는 간헐적인 문제를 파악하는 데 도움이 된다.

제어기는 운용 안전성을 저해하지 않으면서 캘리브레이션(Calibration)과 커미셔닝(Commissioning) 절차도 지원해야 한다. 관절 영점, 엔코더 오프셋, 토크 센서 캘리브레이션, 모터 회전 방향, 전류 제한 및 기계적 동작 범위 파라미터를 저장하고 검증해야 한다. 잘못된 캘리브레이션은 제어 알고리즘 자체가 정상적으로 동작하더라도 물리적으로 위험한 움직임을 발생시킬 수 있으므로 설정 데이터는 버전 관리되고 우발적인 변경으로부터 보호되어야 한다.

기동 순서(Startup Sequencing)는 필요한 구성요소가 유효한 상태에 도달한 이후에만 액추에이터 제어가 시작되도록 한다. 토크를 활성화하기 전에 통신 버스, 센서, 관절 모듈, 캘리브레이션 파라미터, 안전 입력 및 상위 명령 인터페이스를 확인해야 한다. 종료 과정에서도 운용 조건상 제어된 순서가 가능한 경우 액추에이터 전원을 제거하기 전에 로봇을 기계적으로 안정적인 상태로 전환해야 한다.

피지컬 AI(Physical AI) 기능이 확장될수록 AI와 결정론적 제어(Deterministic Control) 사이의 경계는 더욱 중요해진다. 학습 기반 모델은 궤적, 발 디딤 위치, 보행 방식 또는 의미론적 운동 목표를 생성할 수 있지만 이러한 출력은 액추에이터에 전달되기 전에 결정론적인 검증과 제약조건 적용을 거쳐야 한다. 이러한 아키텍처는 확률적인 AI 모델에 안전 핵심 모터 실행에 대한 무제한 권한을 부여하지 않으면서 고급 지능이 로봇 행동에 영향을 줄 수 있도록 한다.

따라서 견고한 실시간 제어기(Real-Time Controller)는 사족보행 로봇 컴퓨팅 아키텍처에서 신뢰할 수 있는 물리적 실행 계층(Trusted Physical Execution Layer)으로 동작한다. 결정론적 스케줄링, 동기화된 통신, 폐루프 제어, 진단, 워치독 및 안전 제약조건을 통해 젯슨 기반 지능과 분산 액추에이터를 연결한다. 젯슨 아키텍처(Jetson Architecture) 및 엣지 GPU 서버(Edge GPU Server)와 함께 이후의 AI 가속(AI Acceleration), PTP 시간 동기화(PTP Time Synchronization), 컴퓨팅 이중화(Compute Redundancy)를 위한 계층형 기반을 구축한다.

##  

## 08.04. AI Acceleration

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

AI acceleration in a quadruped robot provides the computational capability required to execute perception, prediction, semantic understanding, and learned decision functions within practical latency and power limits. Rather than treating AI as a single application, the computing architecture should provide an acceleration pipeline spanning sensor preprocessing, neural-network inference, postprocessing, and delivery of validated results to navigation, planning, and control functions.

The GPU is the primary acceleration resource for highly parallel neural-network workloads. Camera images, LiDAR representations, occupancy grids, feature tensors, and multimodal inputs can be processed simultaneously through thousands of parallel operations. Compared with CPU-only execution, GPU acceleration enables larger models and higher sensor rates while preserving CPU resources for operating-system services, communication, orchestration, diagnostics, and other sequential workloads.

Modern embedded AI platforms may also provide specialized accelerators optimized for neural-network operations. These engines can execute supported layers with improved performance per watt compared with general-purpose GPU execution. Effective architecture therefore does not assume that every model should run on the GPU; workloads should be mapped to CPU, GPU, or dedicated accelerators according to model compatibility, latency requirements, memory behavior, precision, and available power.

The AI software pipeline begins before neural-network inference. Camera decoding, image resizing, normalization, geometric transformation, point-cloud filtering, voxelization, and tensor preparation can consume significant processing time. Hardware-assisted preprocessing and GPU-resident pipelines reduce unnecessary memory transfers. Keeping data close to the accelerator is often as important as increasing raw compute performance because repeated CPU-GPU copies can dominate end-to-end latency.

Inference runtimes convert trained neural networks into optimized execution graphs for the target hardware. Operations may be fused, kernels selected according to tensor dimensions, and memory buffers reused to reduce overhead. TensorRT or comparable optimized runtimes can improve execution efficiency on supported NVIDIA platforms, but acceleration results depend on the actual model structure. Performance should therefore be measured after deployment rather than inferred only from theoretical accelerator specifications.

Numerical precision provides an important tradeoff between model accuracy, memory consumption, and execution speed. FP32 may be useful for development or models sensitive to numerical error, while FP16 or BF16 can reduce memory requirements and increase accelerator utilization. INT8 quantization can provide further efficiency when the model and calibration process support it, but accuracy must be validated against representative robot data before deployment.

AI acceleration must account for multiple models operating simultaneously. A quadruped may execute object detection, terrain classification, depth estimation, localization support, semantic segmentation, anomaly detection, and inspection analytics at the same time. Each model competes for GPU execution time, memory, and bandwidth. Resource planning should therefore evaluate the combined mission workload instead of benchmarking each neural network independently under ideal conditions.

Scheduling becomes more important as the number of AI services increases. Safety-relevant terrain perception may require higher priority and predictable update frequency, while semantic interpretation or inspection analysis may tolerate longer latency. The architecture can assign different execution rates, priorities, batching policies, and resource budgets to each service so that computationally expensive background inference does not degrade functions directly associated with locomotion.

GPU memory capacity establishes a practical limit on model complexity and concurrency. Model parameters occupy only part of the available memory; intermediate activations, input tensors, output buffers, runtime workspaces, temporal states, and other applications also consume capacity. Memory planning should therefore include peak usage under simultaneous operation and sufficient margin for runtime variation rather than selecting hardware based only on model file size.

Temporal AI models introduce additional computational requirements because they process information across multiple time steps. Video models, recurrent networks, world models, and temporal occupancy representations may retain latent states or sequences of features in memory. These models can improve understanding of motion and environmental change, but they increase memory pressure and make predictable scheduling more difficult, particularly on embedded platforms with shared system memory.

The Jetson platform is well suited to AI functions that must remain physically onboard the quadruped. Local inference reduces dependence on external connectivity and enables perception and navigation to continue when communication with an Edge GPU Server is unavailable. Models selected for Jetson should therefore be optimized not only for accuracy but also for sustained thermal performance, power consumption, memory capacity, and worst-case inference latency.

An Edge GPU Server provides a second acceleration tier for models that exceed the practical limits of onboard computing. Large Vision-Language Models, multimodal reasoning systems, dense reconstruction, global mapping, advanced inspection models, and fleet analytics can be offloaded when network conditions permit. This creates a heterogeneous AI architecture in which embedded and server-class accelerators cooperate according to workload characteristics.

Offloading is beneficial only when communication overhead remains compatible with the required response time. Sending raw camera or LiDAR data to an external server may consume substantial bandwidth and introduce variable latency. In some cases, Jetson can perform early feature extraction and transmit compact representations instead. The architecture should compare raw-data transfer, compressed-data transfer, feature transfer, and local inference according to mission requirements.

End-to-end latency is more important than inference latency alone. Sensor exposure, data acquisition, preprocessing, accelerator scheduling, neural-network execution, postprocessing, middleware transport, decision logic, and command generation all contribute to the time between a physical event and robot response. AI acceleration should therefore be evaluated as part of the complete perception-to-action pipeline rather than as an isolated neural-network benchmark.

Performance characterization should include average and tail behavior. P50 latency describes typical operation, while P95 and P99 reveal less frequent delays that may become important during demanding missions. Worst-case behavior should also be examined where practical. GPU utilization, memory usage, thermal state, power consumption, network traffic, and simultaneous workloads should be recorded together so that performance degradation can be traced to its actual cause.

Thermal constraints can convert short benchmark performance into unrealistic expectations. A GPU may sustain maximum frequency during a brief test but throttle after prolonged inference inside a compact quadruped enclosure. AI acceleration must therefore be validated under representative ambient temperature, locomotion load, sensor activity, and mission duration. Cooling design and power modes become part of AI performance engineering rather than independent mechanical considerations.

Model optimization should preserve functional behavior while reducing unnecessary computation. Techniques such as graph optimization, mixed precision, quantization, pruning, input-resolution adjustment, and model distillation can reduce latency or memory requirements. Optimization must be validated using mission-representative datasets because an apparently small accuracy reduction in a generic benchmark may disproportionately affect terrain perception, obstacle recognition, or inspection performance.

AI outputs should carry confidence, timestamps, validity information, and model-version metadata when appropriate. Downstream software must determine whether an inference result is sufficiently recent and trustworthy for use. A technically correct result that arrives too late may be unsafe for motion decisions. Explicit data contracts between AI services, planning software, and deterministic control prevent accelerated inference from becoming an uncontrolled path into physical execution.

Fault handling should detect accelerator overload, memory exhaustion, model failure, thermal throttling, stale inference, and service crashes. The system can respond by reducing inference frequency, disabling lower-priority models, switching to a smaller model, moving selected workloads to another computing node, or entering a degraded autonomous mode. Essential locomotion safety should remain protected by deterministic control functions independent of AI accelerator availability.

As Physical AI evolves, acceleration requirements will expand from conventional perception toward multimodal reasoning, world models, foundation models, and learned planning. These workloads require greater memory, temporal context, and heterogeneous computation. Designing explicit interfaces between Jetson, Edge GPU Servers, and real-time controllers allows computing hardware to evolve without redesigning the complete electrical and control architecture whenever a new AI model is introduced.

AI acceleration is therefore a system-level capability rather than simply the presence of a powerful GPU. Efficient preprocessing, optimized inference, precision management, memory planning, workload scheduling, thermal control, distributed execution, diagnostics, and deterministic interfaces must operate together. Within the quadruped computing architecture, this acceleration layer connects intelligent perception and reasoning to the synchronized, reliable physical execution developed in the surrounding computing subsystems.

사족보행 로봇에서 AI 가속(AI Acceleration)은 실용적인 지연시간과 전력 한계 내에서 인지(Perception), 예측(Prediction), 의미론적 이해(Semantic Understanding), 학습 기반 의사결정(Learned Decision) 기능을 실행하는 데 필요한 연산 능력을 제공한다. AI를 하나의 애플리케이션으로 취급하기보다 센서 전처리, 신경망 추론, 후처리, 검증된 결과를 내비게이션, 경로 계획 및 제어 기능으로 전달하는 전체 과정에 걸친 가속 파이프라인(Acceleration Pipeline)을 제공해야 한다.

GPU는 고도의 병렬 신경망 워크로드를 위한 핵심 가속 자원(Acceleration Resource)이다. 카메라 영상, 라이다 표현(LiDAR Representation), 점유 격자(Occupancy Grid), 특징 텐서(Feature Tensor), 멀티모달 입력을 수천 개의 병렬 연산을 통해 동시에 처리할 수 있다. CPU만을 사용하는 실행 방식과 비교하면 GPU 가속은 더 큰 모델과 높은 센서 처리 주기를 지원하면서 운영체제 서비스, 통신, 오케스트레이션, 진단 및 순차적 워크로드를 위한 CPU 자원을 확보할 수 있게 한다.

현대의 임베디드 AI 플랫폼(Embedded AI Platform)은 신경망 연산에 최적화된 전용 가속기(Specialized Accelerator)를 추가로 제공할 수 있다. 이러한 엔진은 지원되는 연산 계층을 범용 GPU보다 높은 와트당 성능으로 실행할 수 있다. 따라서 모든 모델을 GPU에서 실행한다고 가정하기보다 모델 호환성, 지연시간 요구조건, 메모리 특성, 정밀도 및 사용 가능한 전력을 고려하여 CPU, GPU 또는 전용 가속기에 워크로드를 적절하게 배치해야 한다.

AI 소프트웨어 파이프라인(AI Software Pipeline)은 신경망 추론 이전 단계부터 시작된다. 카메라 디코딩, 영상 크기 조정, 정규화, 기하학적 변환, 포인트 클라우드 필터링, 복셀화(Voxelization), 텐서 준비 과정도 상당한 처리시간을 소비할 수 있다. 하드웨어 가속 전처리와 GPU 상주 파이프라인(GPU-Resident Pipeline)은 불필요한 메모리 전송을 줄인다. 반복적인 CPU-GPU 데이터 복사가 종단간 지연시간을 지배할 수 있으므로 데이터를 가속기 가까이에 유지하는 것은 원시 연산 성능을 높이는 것만큼 중요하다.

추론 런타임(Inference Runtime)은 학습된 신경망을 대상 하드웨어에 최적화된 실행 그래프(Execution Graph)로 변환한다. 연산을 융합하고 텐서 크기에 적합한 커널을 선택하며 메모리 버퍼를 재사용하여 오버헤드를 줄일 수 있다. TensorRT 또는 이와 유사한 최적화 런타임은 지원되는 NVIDIA 플랫폼에서 실행 효율을 향상시킬 수 있지만 가속 효과는 실제 모델 구조에 따라 달라진다. 따라서 성능은 이론적인 가속기 사양만으로 판단하지 않고 실제 배포 이후 측정해야 한다.

수치 정밀도(Numerical Precision)는 모델 정확도, 메모리 사용량 및 실행 속도 사이의 중요한 절충 요소이다. FP32는 개발 단계나 수치 오차에 민감한 모델에서 사용할 수 있으며, FP16 또는 BF16은 메모리 요구량을 줄이고 가속기 활용률을 높일 수 있다. INT8 양자화(INT8 Quantization)는 모델과 캘리브레이션 과정이 지원하는 경우 추가적인 효율 향상을 제공하지만 배포 전에 실제 로봇을 대표하는 데이터를 사용하여 정확도를 검증해야 한다.

AI 가속은 여러 모델이 동시에 동작하는 상황을 고려해야 한다. 사족보행 로봇은 객체 감지(Object Detection), 지형 분류, 깊이 추정, 위치추정 지원, 의미론적 분할(Semantic Segmentation), 이상 감지 및 검사 분석을 동시에 실행할 수 있다. 각 모델은 GPU 실행시간, 메모리 및 대역폭을 서로 경쟁하여 사용한다. 따라서 각각의 신경망을 이상적인 조건에서 독립적으로 벤치마킹하기보다 전체 임무 워크로드가 동시에 실행되는 상황을 기준으로 자원을 계획해야 한다.

AI 서비스의 수가 증가할수록 스케줄링(Scheduling)의 중요성도 높아진다. 안전과 관련된 지형 인지는 높은 우선순위와 예측 가능한 갱신 주기가 필요하지만 의미론적 해석이나 검사 분석은 상대적으로 긴 지연시간을 허용할 수 있다. 각 서비스에 서로 다른 실행 주기, 우선순위, 배칭 정책(Batching Policy), 자원 예산을 할당하여 연산량이 큰 백그라운드 추론이 보행과 직접적으로 관련된 기능의 성능을 저하시키지 않도록 해야 한다.

GPU 메모리 용량은 모델 복잡성과 동시 실행에 대한 실질적인 한계를 결정한다. 모델 파라미터는 전체 메모리의 일부만 차지하며 중간 활성값(Intermediate Activation), 입력 텐서, 출력 버퍼, 런타임 작업 공간, 시간 상태(Temporal State) 및 다른 애플리케이션도 메모리를 사용한다. 따라서 하드웨어 선정 시 모델 파일 크기만을 기준으로 하지 않고 동시 실행 조건에서의 최대 메모리 사용량과 런타임 변동을 위한 충분한 여유 공간을 고려해야 한다.

시간 기반 AI 모델(Temporal AI Model)은 여러 시간 단계의 정보를 처리하므로 추가적인 연산 요구조건을 발생시킨다. 비디오 모델, 순환 신경망(Recurrent Network), 월드 모델(World Model), 시간 점유 표현(Temporal Occupancy Representation)은 잠재 상태(Latent State) 또는 특징 시퀀스를 메모리에 유지할 수 있다. 이러한 모델은 움직임과 환경 변화에 대한 이해를 향상시키지만 메모리 부담을 증가시키며, 특히 공유 시스템 메모리를 사용하는 임베디드 플랫폼에서 예측 가능한 스케줄링을 더욱 어렵게 만든다.

젯슨 플랫폼(Jetson Platform)은 사족보행 로봇에 물리적으로 탑재되어야 하는 AI 기능에 적합하다. 로컬 추론(Local Inference)은 외부 연결에 대한 의존성을 줄이고 엣지 GPU 서버(Edge GPU Server)와의 통신이 불가능한 상황에서도 인지와 내비게이션을 지속할 수 있게 한다. 따라서 젯슨용 모델은 정확도뿐만 아니라 지속적인 열 성능, 전력 소비, 메모리 용량 및 최악조건 추론 지연시간(Worst-Case Inference Latency)을 고려하여 최적화해야 한다.

엣지 GPU 서버(Edge GPU Server)는 온보드 컴퓨팅의 실질적인 한계를 초과하는 모델을 위한 두 번째 가속 계층(Acceleration Tier)을 제공한다. 대규모 비전-언어 모델(Vision-Language Model), 멀티모달 추론 시스템, 고밀도 재구성, 전역 지도작성, 고급 검사 모델 및 플릿 분석(Fleet Analytics)은 네트워크 조건이 허용하는 경우 서버로 오프로딩(Offloading)할 수 있다. 이를 통해 임베디드 가속기와 서버급 가속기가 워크로드 특성에 따라 협력하는 이기종 AI 아키텍처(Heterogeneous AI Architecture)를 구성할 수 있다.

오프로딩은 통신 오버헤드가 요구되는 응답시간과 양립할 수 있을 때만 효과적이다. 원시 카메라 또는 라이다 데이터를 외부 서버로 전송하면 상당한 대역폭을 소비하고 가변적인 지연시간을 발생시킬 수 있다. 경우에 따라 젯슨에서 초기 특징 추출(Early Feature Extraction)을 수행하고 압축된 표현을 전송할 수 있다. 아키텍처는 임무 요구조건에 따라 원시 데이터, 압축 데이터, 특징 데이터 전송 및 로컬 추론 방식을 비교해야 한다.

종단간 지연시간(End-to-End Latency)은 추론 지연시간만을 측정하는 것보다 중요하다. 센서 노출, 데이터 취득, 전처리, 가속기 스케줄링, 신경망 실행, 후처리, 미들웨어 전송, 의사결정 로직 및 명령 생성까지 모두 물리적 사건이 발생한 시점부터 로봇이 반응하기까지의 시간에 영향을 준다. 따라서 AI 가속은 독립적인 신경망 벤치마크가 아니라 전체 인지-행동 파이프라인(Perception-to-Action Pipeline)의 일부로 평가해야 한다.

성능 특성화(Performance Characterization)에서는 평균값뿐만 아니라 꼬리 지연 특성(Tail Behavior)도 포함해야 한다. P50 지연시간은 일반적인 동작을 나타내며 P95와 P99는 고부하 임무에서 중요해질 수 있는 드문 지연을 보여준다. 가능한 경우 최악조건 동작도 평가해야 한다. GPU 사용률, 메모리 사용량, 열 상태, 전력 소비, 네트워크 트래픽 및 동시 워크로드를 함께 기록하면 성능 저하의 실제 원인을 추적할 수 있다.

열적 제약조건(Thermal Constraint)은 짧은 벤치마크에서 측정된 성능을 실제 운용에서 달성하기 어렵게 만들 수 있다. GPU는 짧은 시험에서는 최대 주파수를 유지하지만 소형 사족보행 로봇의 밀폐된 공간에서 장시간 추론을 수행하면 열 스로틀링(Thermal Throttling)이 발생할 수 있다. 따라서 실제 주변 온도, 보행 부하, 센서 동작 및 임무 지속시간을 반영하여 AI 가속을 검증해야 하며 냉각 설계와 전력 모드도 AI 성능 엔지니어링의 일부로 다루어야 한다.

모델 최적화(Model Optimization)는 기능적 동작을 유지하면서 불필요한 연산을 줄여야 한다. 그래프 최적화(Graph Optimization), 혼합 정밀도(Mixed Precision), 양자화(Quantization), 가지치기(Pruning), 입력 해상도 조정 및 모델 증류(Model Distillation) 등의 기법으로 지연시간이나 메모리 요구량을 줄일 수 있다. 일반 벤치마크에서 작은 정확도 감소가 지형 인지, 장애물 인식 또는 검사 성능에는 큰 영향을 줄 수 있으므로 임무를 대표하는 데이터셋으로 최적화 결과를 검증해야 한다.

AI 출력에는 필요한 경우 신뢰도(Confidence), 타임스탬프, 유효성 정보 및 모델 버전 메타데이터(Model-Version Metadata)를 포함해야 한다. 하위 소프트웨어는 추론 결과가 사용하기에 충분히 최신이고 신뢰할 수 있는지를 판단해야 한다. 기술적으로 정확한 결과라도 지나치게 늦게 도착하면 운동 결정에 안전하게 사용할 수 없다. AI 서비스, 경로 계획 소프트웨어 및 결정론적 제어 사이의 명확한 데이터 계약(Data Contract)은 가속된 추론 결과가 통제되지 않은 상태로 물리적 실행에 전달되는 것을 방지한다.

고장 처리(Fault Handling)는 가속기 과부하, 메모리 고갈, 모델 실패, 열 스로틀링, 오래된 추론 결과 및 서비스 충돌을 감지해야 한다. 시스템은 추론 주기를 낮추거나 우선순위가 낮은 모델을 비활성화하고, 더 작은 모델로 전환하거나, 일부 워크로드를 다른 컴퓨팅 노드로 이동하거나, 성능 저하 자율 모드(Degraded Autonomous Mode)로 전환할 수 있다. 필수적인 보행 안전은 AI 가속기의 가용성과 독립적인 결정론적 제어 기능에 의해 보호되어야 한다.

피지컬 AI(Physical AI)가 발전함에 따라 가속 요구사항은 기존의 인지 기능에서 멀티모달 추론, 월드 모델(World Model), 파운데이션 모델(Foundation Model), 학습 기반 경로 계획(Learned Planning)으로 확대될 것이다. 이러한 워크로드는 더 큰 메모리, 더 긴 시간 문맥(Temporal Context), 이기종 연산(Heterogeneous Computation)을 필요로 한다. 젯슨, 엣지 GPU 서버 및 실시간 제어기 사이에 명확한 인터페이스를 설계하면 새로운 AI 모델이 도입될 때마다 전체 전기 및 제어 아키텍처를 재설계하지 않고 컴퓨팅 하드웨어를 발전시킬 수 있다.

따라서 AI 가속(AI Acceleration)은 단순히 강력한 GPU가 존재하는 것이 아니라 시스템 수준의 능력(System-Level Capability)으로 이해해야 한다. 효율적인 전처리, 최적화된 추론, 정밀도 관리, 메모리 계획, 워크로드 스케줄링, 열 제어, 분산 실행, 진단 및 결정론적 인터페이스가 함께 동작해야 한다. 사족보행 로봇 컴퓨팅 아키텍처에서 이러한 가속 계층은 지능형 인지 및 추론을 주변 컴퓨팅 서브시스템이 제공하는 동기화되고 신뢰할 수 있는 물리적 실행과 연결한다.

##  

## 08.05. PTP Time Synchronization

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

PTP time synchronization provides a common temporal reference across the distributed computing, sensing, and control components of a quadruped robot. Cameras, LiDAR, IMU, GNSS, Jetson computers, Edge GPU Servers, and real-time controllers may each contain independent oscillators. Without synchronization, their timestamps gradually diverge, making observations of the same physical event appear to have occurred at different times.

Precise timing is especially important because quadruped locomotion produces rapid changes in body orientation, joint position, foot contact, and sensor viewpoint. A camera image acquired only milliseconds away from the corresponding IMU or joint state can represent a measurably different robot configuration. Synchronization therefore affects sensor fusion, localization, state estimation, mapping, perception, and the ability to associate environmental observations with physical motion.

Precision Time Protocol, commonly implemented according to IEEE 1588, distributes time over an Ethernet network by exchanging timestamped synchronization messages between clocks. Instead of merely setting clocks occasionally, PTP continuously estimates timing relationships and compensates for clock offset and frequency differences. This allows distributed devices to maintain a shared representation of time while continuing to operate with their own local hardware clocks.

A PTP domain typically establishes a reference clock from which other participating clocks derive synchronized time. The reference may originate from a dedicated timing device, GNSS-disciplined source, industrial computer, or another appropriately configured clock. The system architecture should define clock authority explicitly so that startup, reference loss, recovery, and transitions between available timing sources produce predictable behavior.

Hardware timestamping significantly improves synchronization quality because timestamps are captured close to the physical network interface rather than after unpredictable operating-system and software delays. Software timestamps may include scheduling, buffering, and driver latency that varies from packet to packet. Hardware-assisted PTP therefore provides a more stable foundation when accurate correlation between high-rate sensors and real-time control information is required.

Network infrastructure influences achievable synchronization accuracy. Ethernet switches located between the reference clock and synchronized devices introduce forwarding delays and variations. PTP-aware switches can support timing mechanisms that account for these effects, while ordinary network equipment may provide less predictable results. Timing architecture should consequently be considered during switch selection rather than treated purely as a software configuration issue.

PTP message exchange enables a device to estimate the offset between its local clock and the reference clock as well as the propagation delay through the communication path. A clock-control mechanism then adjusts local timing gradually to reduce the measured error. Sudden uncontrolled time jumps should be avoided where they could disturb logging, sensor fusion, or real-time software that assumes monotonically progressing timestamps.

Sensor timestamp provenance is as important as clock synchronization itself. A timestamp should represent when physical data was acquired, not merely when a software process received or processed it. Camera exposure time, LiDAR measurement time, IMU sampling time, and encoder acquisition time may occur significantly before application-level processing. Preserving the original acquisition timestamp allows downstream algorithms to reconstruct the actual temporal relationship between measurements.

Camera synchronization requires particular attention because image exposure and frame delivery are different events. A frame may be timestamped at exposure start, exposure completion, hardware trigger, or driver reception depending on the sensor architecture. The selected convention must be documented and maintained consistently. Where tighter alignment is required, hardware trigger signals can coordinate exposure while PTP provides the common time reference used to identify each frame.

LiDAR sensors similarly generate measurements over finite acquisition intervals rather than at a single instant. A rotating LiDAR may collect points while both the environment and robot are moving. Accurate timestamps enable motion compensation by associating portions of a scan with the corresponding robot pose. Poor synchronization can distort reconstructed geometry and degrade localization even when the LiDAR itself provides highly accurate range measurements.

IMU synchronization is critical because inertial measurements are commonly produced at higher rates than cameras or LiDAR. Orientation and angular-rate information can be interpolated to the acquisition time of slower sensors when all measurements share a common clock reference. This allows camera frames, point clouds, and joint states to be compensated for robot motion instead of being combined according to arrival order at the computing node.

The real-time controller also participates in the temporal architecture by timestamping joint states, actuator feedback, foot-contact information, and control events. High-level computing can then associate a perception result with the actual mechanical configuration of the robot at the relevant instant. This distinction becomes essential when network and processing latency cause sensor information to arrive substantially later than the physical event it represents.

Jetson acts as an important integration point because it receives data from multiple sensors while exchanging state and commands with the real-time controller. Its software should preserve source timestamps throughout ROS 2 or other middleware pipelines rather than replacing them with local reception time. Queues, preprocessing stages, inference services, and message conversions should maintain timestamp metadata so temporal information survives the complete perception pipeline.

An Edge GPU Server introduces another timing boundary because sensor data may cross a network before AI processing occurs. The server should operate within the same synchronized time domain where practical and retain original sensor timestamps. AI results can additionally include processing timestamps and validity information, allowing Jetson to distinguish when the physical observation occurred from when inference completed and when the result was received.

Time synchronization and communication determinism are related but different concepts. PTP aligns clocks, but it does not guarantee that Ethernet packets will arrive within a fixed latency. Network congestion can delay a correctly timestamped message without changing the time represented by its timestamp. System design must therefore manage both clock synchronization and network quality when functions depend on bounded response times.

Monitoring is necessary because synchronization quality can deteriorate without causing obvious communication failure. Useful diagnostics include clock offset, path delay, synchronization state, reference-clock identity, frequency adjustment, message loss, and transitions between timing sources. These parameters should be logged together with robot events so that localization or perception anomalies can later be correlated with timing degradation.

Loss of the PTP reference should produce a defined response rather than an immediate assumption that all timestamps are invalid. Local clocks can continue operating in a holdover state for a limited period, although their error gradually increases according to oscillator quality. Software can expose synchronization status and estimated timing uncertainty so that applications decide whether degraded timing remains acceptable for the current function.

Safety-related functions should not depend exclusively on network time synchronization where an independent local timing mechanism can provide the necessary protection. Emergency stop, actuator current limits, watchdogs, and critical real-time control deadlines should continue functioning with local deterministic clocks. PTP supports coordination across distributed components, but fundamental protection mechanisms should remain robust when the timing network becomes unavailable.

Startup sequencing should verify timing readiness before enabling functions that require synchronized sensor fusion. Devices need time to discover the reference, estimate network delay, and converge toward acceptable clock offset. The robot may allow basic diagnostics during this period while delaying high-precision localization or autonomous movement until synchronization status satisfies predefined acceptance criteria.

Validation should measure synchronization under realistic operating conditions rather than only on an idle laboratory network. Testing should include high sensor bandwidth, concurrent AI traffic, logging, multiple computing nodes, switch loading, startup transitions, reference-clock loss, and recovery. Offset distributions and worst observed timing errors are more informative than a single nominal synchronization value when evaluating robustness.

PTP time synchronization ultimately creates a shared temporal coordinate system for the quadruped robot. When acquisition timestamps, hardware clocks, network timing, middleware metadata, and diagnostics are engineered consistently, distributed sensor and control data can be interpreted according to when events actually occurred. This common time foundation connects Jetson computing, Edge GPU processing, real-time control, and later compute-redundancy mechanisms into a coherent physical system.

PTP 시간 동기화(PTP Time Synchronization)는 사족보행 로봇의 분산 컴퓨팅, 센싱 및 제어 구성요소 전체에 공통 시간 기준(Common Temporal Reference)을 제공한다. 카메라, 라이다(LiDAR), IMU, GNSS, 젯슨(Jetson) 컴퓨터, 엣지 GPU 서버(Edge GPU Server), 실시간 제어기(Real-Time Controller)는 각각 독립적인 발진기(Oscillator)를 가질 수 있다. 동기화가 없으면 각 장치의 타임스탬프가 점차 서로 어긋나 동일한 물리적 사건이 서로 다른 시점에 발생한 것처럼 나타날 수 있다.

정밀한 시간 정보는 사족보행 로봇의 움직임으로 몸체 자세, 관절 위치, 발 접촉 상태 및 센서 시점이 빠르게 변화하기 때문에 특히 중요하다. 대응되는 IMU 또는 관절 상태와 불과 수 밀리초 차이로 취득된 카메라 영상도 측정 가능한 수준으로 다른 로봇 자세를 나타낼 수 있다. 따라서 시간 동기화는 센서 융합, 위치추정, 상태 추정, 지도작성, 인지 및 환경 관측과 물리적 운동을 정확하게 연계하는 능력에 직접적인 영향을 준다.

정밀 시간 프로토콜(Precision Time Protocol, PTP)은 일반적으로 IEEE 1588에 따라 구현되며 클록(Clock) 사이에서 타임스탬프가 포함된 동기화 메시지를 교환하여 이더넷 네트워크를 통해 시간을 분배한다. PTP는 단순히 클록을 주기적으로 맞추는 것이 아니라 시간 관계를 지속적으로 추정하고 클록 오프셋(Clock Offset)과 주파수 차이를 보정한다. 이를 통해 분산 장치가 자체 로컬 하드웨어 클록을 사용하면서도 공통된 시간 표현을 유지할 수 있다.

PTP 도메인(PTP Domain)은 일반적으로 다른 참여 클록이 동기화된 시간을 도출할 수 있는 기준 클록(Reference Clock)을 설정한다. 기준은 전용 타이밍 장치, GNSS 동기화 소스(GNSS-Disciplined Source), 산업용 컴퓨터 또는 적절하게 구성된 다른 클록에서 제공될 수 있다. 시스템 아키텍처에서는 클록 권한(Clock Authority)을 명확하게 정의하여 기동, 기준 소실, 복구 및 사용 가능한 시간 소스 사이의 전환이 예측 가능하게 이루어지도록 해야 한다.

하드웨어 타임스탬핑(Hardware Timestamping)은 예측하기 어려운 운영체제 및 소프트웨어 지연 이후가 아니라 물리적 네트워크 인터페이스에 가까운 위치에서 타임스탬프를 기록하므로 동기화 품질을 크게 향상시킨다. 소프트웨어 타임스탬프에는 패킷마다 달라지는 스케줄링, 버퍼링 및 드라이버 지연이 포함될 수 있다. 따라서 고속 센서와 실시간 제어 정보 사이의 정확한 시간 연계가 필요한 경우 하드웨어 지원 PTP가 더욱 안정적인 기반을 제공한다.

네트워크 인프라(Network Infrastructure)는 달성 가능한 동기화 정확도에 영향을 준다. 기준 클록과 동기화 대상 장치 사이에 위치한 이더넷 스위치는 패킷 전달 지연과 변동을 발생시킨다. PTP 인식 스위치(PTP-Aware Switch)는 이러한 영향을 고려하는 타이밍 메커니즘을 지원할 수 있지만 일반 네트워크 장비에서는 결과가 상대적으로 예측하기 어려울 수 있다. 따라서 타이밍 아키텍처는 단순한 소프트웨어 설정 문제가 아니라 네트워크 스위치 선정 단계부터 고려해야 한다.

PTP 메시지 교환을 통해 장치는 로컬 클록과 기준 클록 사이의 오프셋뿐만 아니라 통신 경로의 전파 지연(Propagation Delay)을 추정할 수 있다. 이후 클록 제어 메커니즘(Clock-Control Mechanism)이 측정된 오차를 줄이도록 로컬 시간을 점진적으로 조정한다. 로깅, 센서 융합 또는 시간이 단조롭게 증가한다고 가정하는 실시간 소프트웨어에 영향을 줄 수 있으므로 제어되지 않은 급격한 시간 점프(Time Jump)는 피해야 한다.

센서 타임스탬프 출처(Sensor Timestamp Provenance)는 클록 동기화 자체만큼 중요하다. 타임스탬프는 소프트웨어 프로세스가 데이터를 수신하거나 처리한 시간이 아니라 실제 물리적 데이터가 취득된 시점을 나타내야 한다. 카메라 노출 시점, 라이다 측정 시점, IMU 샘플링 시점 및 엔코더 취득 시점은 애플리케이션 수준의 처리보다 상당히 앞설 수 있다. 원래의 취득 타임스탬프를 보존하면 하위 알고리즘에서 측정값 사이의 실제 시간 관계를 재구성할 수 있다.

카메라 동기화(Camera Synchronization)는 영상 노출과 프레임 전달이 서로 다른 사건이므로 특별한 주의가 필요하다. 센서 아키텍처에 따라 프레임은 노출 시작, 노출 완료, 하드웨어 트리거(Hardware Trigger) 또는 드라이버 수신 시점을 기준으로 타임스탬프를 기록할 수 있다. 선택된 기준은 명확하게 문서화하고 일관되게 유지해야 한다. 더 정밀한 정렬이 필요한 경우 하드웨어 트리거로 노출 시점을 조정하고 PTP를 각 프레임을 식별하는 공통 시간 기준으로 사용할 수 있다.

라이다 센서 역시 하나의 순간이 아니라 일정한 취득 구간(Acquisition Interval)에 걸쳐 측정값을 생성한다. 회전형 라이다(Rotating LiDAR)는 환경과 로봇이 모두 움직이는 동안 포인트를 수집할 수 있다. 정확한 타임스탬프를 이용하면 스캔의 각 부분을 해당 시점의 로봇 자세와 연계하여 운동 보상(Motion Compensation)을 수행할 수 있다. 동기화가 부정확하면 라이다 자체의 거리 측정 정확도가 높더라도 재구성된 형상이 왜곡되고 위치추정 성능이 저하될 수 있다.

IMU 동기화(IMU Synchronization)는 관성 측정값이 일반적으로 카메라나 라이다보다 높은 주기로 생성되기 때문에 중요하다. 모든 측정값이 공통 클록 기준을 공유하면 방향과 각속도 정보를 상대적으로 느린 센서의 취득 시점에 맞추어 보간(Interpolation)할 수 있다. 이를 통해 카메라 프레임, 포인트 클라우드 및 관절 상태를 컴퓨팅 노드에 도착한 순서가 아니라 실제 로봇 운동을 기준으로 보정하고 결합할 수 있다.

실시간 제어기(Real-Time Controller) 역시 관절 상태, 액추에이터 피드백, 발 접촉 정보 및 제어 이벤트에 타임스탬프를 기록함으로써 시간 아키텍처에 참여한다. 이를 통해 상위 컴퓨팅 계층은 인지 결과를 해당 시점의 실제 로봇 기계적 상태와 연계할 수 있다. 네트워크와 처리 지연으로 센서 정보가 실제 물리적 사건보다 상당히 늦게 도착하는 경우 이러한 구분은 특히 중요해진다.

젯슨(Jetson)은 여러 센서로부터 데이터를 수신하면서 실시간 제어기와 상태 및 명령을 교환하므로 중요한 통합 지점(Integration Point)으로 동작한다. 젯슨의 소프트웨어는 ROS 2 또는 다른 미들웨어 파이프라인을 통과하는 동안 소스 타임스탬프(Source Timestamp)를 로컬 수신 시각으로 대체하지 않고 보존해야 한다. 큐(Queue), 전처리 단계, 추론 서비스 및 메시지 변환에서도 타임스탬프 메타데이터를 유지하여 전체 인지 파이프라인에서 시간 정보가 보존되도록 해야 한다.

엣지 GPU 서버(Edge GPU Server)는 AI 처리가 수행되기 전에 센서 데이터가 네트워크를 통과할 수 있으므로 또 하나의 시간 경계(Timing Boundary)를 형성한다. 가능한 경우 서버도 동일한 동기화 시간 도메인에서 동작하고 원래의 센서 타임스탬프를 유지해야 한다. AI 결과에는 처리 타임스탬프와 유효성 정보를 추가하여 젯슨이 물리적 관측 시점, 추론 완료 시점 및 결과 수신 시점을 서로 구분할 수 있도록 할 수 있다.

시간 동기화(Time Synchronization)와 통신 결정성(Communication Determinism)은 서로 관련되어 있지만 다른 개념이다. PTP는 클록을 정렬하지만 이더넷 패킷이 고정된 지연시간 내에 도착하는 것을 보장하지 않는다. 네트워크 혼잡은 정확한 타임스탬프가 기록된 메시지의 전달을 지연시킬 수 있지만 타임스탬프가 나타내는 실제 시간 자체를 변경하지는 않는다. 따라서 응답시간 제한이 필요한 기능에서는 클록 동기화와 네트워크 품질을 함께 관리해야 한다.

동기화 품질은 명확한 통신 장애 없이도 저하될 수 있으므로 모니터링(Monitoring)이 필요하다. 유용한 진단 항목에는 클록 오프셋, 경로 지연(Path Delay), 동기화 상태, 기준 클록 식별 정보, 주파수 보정값, 메시지 손실 및 시간 소스 전환 등이 포함된다. 이러한 파라미터를 로봇 이벤트와 함께 기록하면 이후 위치추정이나 인지 이상 현상이 시간 동기화 품질 저하와 관련되었는지를 분석할 수 있다.

PTP 기준 소스가 상실되더라도 모든 타임스탬프가 즉시 무효라고 판단하기보다 정의된 대응 절차를 수행해야 한다. 로컬 클록은 일정 기간 홀드오버 상태(Holdover State)로 계속 동작할 수 있지만 발진기 품질에 따라 시간 오차가 점차 증가한다. 소프트웨어는 동기화 상태와 추정 시간 불확실성(Estimated Timing Uncertainty)을 제공하여 각 애플리케이션이 현재 기능에서 성능이 저하된 시간 정보의 사용 가능 여부를 판단할 수 있도록 해야 한다.

안전 관련 기능(Safety-Related Function)은 독립적인 로컬 타이밍 메커니즘으로 필요한 보호 기능을 제공할 수 있는 경우 네트워크 시간 동기화에만 의존해서는 안 된다. 비상 정지, 액추에이터 전류 제한, 워치독(Watchdog), 핵심 실시간 제어 마감시간은 로컬 결정론적 클록(Local Deterministic Clock)을 통해 계속 동작해야 한다. PTP는 분산 구성요소 사이의 협조를 지원하지만 타이밍 네트워크가 사용할 수 없는 경우에도 기본적인 보호 기능은 견고하게 유지되어야 한다.

기동 순서(Startup Sequencing)에서는 동기화된 센서 융합을 필요로 하는 기능을 활성화하기 전에 시간 동기화 준비 상태(Timing Readiness)를 확인해야 한다. 장치가 기준 클록을 발견하고 네트워크 지연을 추정하며 허용 가능한 클록 오프셋으로 수렴하는 데 시간이 필요하다. 이 기간에는 기본적인 진단 기능을 허용하면서 고정밀 위치추정이나 자율 이동은 동기화 상태가 사전에 정의된 허용 기준을 만족할 때까지 지연시킬 수 있다.

검증(Validation)은 유휴 상태의 실험실 네트워크에서만 수행하지 않고 실제 운용 조건에서 동기화 성능을 측정해야 한다. 시험에는 높은 센서 대역폭, 동시 AI 트래픽, 로깅, 다수의 컴퓨팅 노드, 스위치 부하, 기동 전환, 기준 클록 소실 및 복구 조건이 포함되어야 한다. 견고성을 평가할 때 단일 명목 동기화 값보다 클록 오프셋의 분포와 관측된 최악조건 시간 오차가 더욱 유용하다.

궁극적으로 PTP 시간 동기화(PTP Time Synchronization)는 사족보행 로봇을 위한 공유 시간 좌표계(Shared Temporal Coordinate System)를 형성한다. 취득 타임스탬프, 하드웨어 클록, 네트워크 타이밍, 미들웨어 메타데이터 및 진단 기능을 일관되게 설계하면 분산된 센서 및 제어 데이터를 실제 사건이 발생한 시점을 기준으로 해석할 수 있다. 이러한 공통 시간 기반은 젯슨 컴퓨팅, 엣지 GPU 처리, 실시간 제어 및 이후의 컴퓨팅 이중화(Compute Redundancy) 메커니즘을 하나의 일관된 물리 시스템으로 연결한다.

##  

## 08.06. Compute Redundancy

![](images/image6.png){width="7.268055555555556in" height="7.268055555555556in"}

Compute redundancy in a quadruped robot provides continued control, fault containment, and safe degradation when a computing component becomes unavailable or produces unreliable results. Because locomotion depends on coordinated perception, state estimation, planning, and deterministic joint control, a single computing failure can affect the entire robot. Redundancy therefore protects critical computational functions rather than simply duplicating processors.

The redundancy architecture should begin by identifying which functions must survive particular failures. Emergency protection and actuator safety require stronger independence than noncritical inspection analytics, while basic locomotion may need to continue even if advanced AI becomes unavailable. This functional classification determines where duplication, fallback processing, independent monitoring, or controlled shutdown is appropriate.

A hierarchical quadruped architecture naturally provides several levels of computational independence. The Edge GPU Server can execute high-performance AI workloads, Jetson can maintain onboard autonomy, and the real-time controller can preserve deterministic motion and safety functions. Failure of one layer should therefore reduce available capability according to a defined degradation strategy rather than automatically causing uncontrolled loss of the complete system.

Redundancy can be implemented through active-active, active-standby, or functionally diverse configurations. Active-active nodes execute functions simultaneously and can compare outputs or distribute workloads. Active-standby architectures maintain a secondary node capable of taking over after primary failure. Functional diversity instead uses different computing paths or algorithms to reduce the probability that one common failure mechanism disables all available channels.

Complete hardware duplication is not always necessary or desirable. A quadruped has strict constraints on mass, volume, power consumption, thermal dissipation, wiring, and cost. Redundancy should therefore be selective. Critical state estimation, command supervision, safety monitoring, and essential navigation may justify duplicated resources, while large VLM inference or inspection analytics can often be temporarily removed without preventing the robot from reaching a safe state.

The Jetson computing layer is a key candidate for redundancy because it integrates perception, localization, navigation, mission logic, and communication with lower-level control. A secondary Jetson or another independent compute node can maintain essential services or receive replicated state information. The backup does not necessarily require identical performance if its purpose is to provide reduced-speed navigation, controlled retreat, or safe mission termination.

An Edge GPU Server provides another form of computational redundancy when selected AI services can execute either onboard or externally. Heavy inference may normally run on the server while a smaller fallback model remains available on Jetson. Conversely, server-side processing can temporarily replace a failed or overloaded onboard AI service when communication conditions permit. Such redundancy combines workload portability with fault recovery rather than relying only on identical hardware.

The real-time controller must remain sufficiently independent from high-level computing failures. If Jetson crashes or becomes unresponsive, joint control loops, actuator limits, watchdogs, and essential safety functions should continue long enough to execute a predefined safe response. This separation prevents failure of Linux, ROS 2, GPU software, or AI inference from directly eliminating deterministic motor-control authority.

Health monitoring is necessary to determine whether a computing node is truly available. Heartbeat messages can detect complete loss of communication, but they are insufficient for identifying incorrect computation. Monitoring should also examine task execution, data freshness, timing behavior, memory health, temperature, communication status, synchronization state, and application-level plausibility to detect nodes that remain online but no longer provide trustworthy results.

Watchdogs provide a local mechanism for detecting processor or software failure. Independent hardware watchdogs can supervise critical execution and trigger reset or isolation when expected activity disappears. Software watchdogs can additionally monitor individual processes and services. Redundant architectures should avoid having the same failed processor responsible for both performing a critical function and deciding whether that function itself is healthy.

Failover requires reliable detection before authority is transferred. If both primary and backup computers believe they control the same actuator or mission function simultaneously, conflicting commands can occur. The architecture therefore requires explicit ownership, arbitration, and state-transition mechanisms. Control authority should move between nodes according to deterministic rules, with clear handling of startup, recovery, communication loss, and ambiguous node states.

State synchronization determines how quickly a standby node can assume responsibility. A backup processor may require current robot pose, map state, mission progress, joint condition, calibration information, and recent sensor history. Continuously replicating every internal state can be expensive and create tight software coupling, so the architecture should define a minimal recoverable state that allows the backup to resume essential operation safely.

Time synchronization is fundamental to redundant computation because outputs from different nodes must be compared against a common temporal reference. PTP-synchronized clocks allow the system to determine whether two estimates correspond to the same physical instant. Without common time, differences caused by measurement timing or network delay may be incorrectly interpreted as computational disagreement, making fault detection and voting less reliable.

Data-path redundancy must also be considered because duplicated computers connected through a single switch, power converter, or communication cable can still share a single point of failure. Critical channels may require independent network paths, separate switch ports, redundant power feeds, or alternative communication buses. Redundancy analysis should therefore examine the entire dependency chain rather than counting the number of processors.

Power architecture is closely connected to compute redundancy. Two computers powered by the same unprotected converter may fail simultaneously during a power disturbance. Critical redundant nodes can use independently protected power branches or appropriately separated DC-DC conversion paths. Power-good signals, undervoltage monitoring, controlled startup, and independent shutdown mechanisms help distinguish computing faults from underlying electrical failures.

Thermal design can also create common-cause failures. Two redundant processors installed in the same poorly cooled enclosure may simultaneously throttle or shut down during high ambient temperature. Physical placement, heat paths, airflow, and thermal monitoring should therefore be evaluated as part of redundancy design. Logical duplication provides little protection when both channels depend on the same inadequate environmental condition.

Network partitioning introduces another failure mode. A healthy computing node may become isolated from sensors or controllers even though its processor remains operational. Redundancy management must distinguish processor failure from communication-path failure and determine which node still has access to the required information. Network topology and service placement should be designed so that essential functions do not depend on one unnecessary communication bottleneck.

Redundant perception does not necessarily require identical sensors or identical algorithms. Camera, LiDAR, IMU, encoder, and foot-force information provide different observations of robot state and environment. Cross-checking independent modalities can detect implausible results that processor duplication alone cannot identify. Functional diversity is particularly valuable when software defects or corrupted input data could affect identical redundant pipelines in the same manner.

Degraded operation should be explicitly defined as part of the redundancy strategy. Loss of an Edge GPU Server may disable large-model reasoning while preserving normal locomotion. Loss of one perception service may reduce speed or restrict terrain. Loss of primary Jetson computing may trigger backup navigation or controlled stopping. Failure of critical real-time control may instead require immediate transition toward the safest achievable physical state.

Recovery after a fault must be controlled as carefully as failover. A rebooted node should not automatically regain authority merely because it is communicating again. It must restore configuration, synchronize time, verify sensor and network access, recover required state, and demonstrate healthy execution. Only after these conditions are satisfied should the redundancy manager permit reintegration or transfer of responsibility.

Diagnostics and event logging are essential for understanding redundancy behavior in field operation. Failover cause, node health, watchdog events, timing offsets, communication errors, power conditions, thermal state, authority transitions, and recovery actions should be timestamped and recorded. These records allow engineers to determine whether redundancy responded to a genuine hardware failure, software defect, network problem, or false-positive health decision.

Validation must deliberately inject failures instead of assuming redundant hardware guarantees reliability. Tests should include processor crashes, application hangs, network disconnection, delayed messages, corrupted data, loss of PTP synchronization, GPU overload, thermal throttling, power interruption, and recovery. The important measurement is not merely whether a backup exists, but whether the robot detects the fault and reaches the intended operational state within the required time.

Compute redundancy ultimately creates a layered resilience architecture for the quadruped robot. Edge GPU resources, Jetson computing, real-time controllers, communication networks, synchronized time, power distribution, watchdogs, and safety mechanisms must cooperate without introducing hidden common points of failure. Properly engineered redundancy allows advanced AI capabilities to fail gracefully while preserving deterministic control and the safest achievable physical behavior.

사족보행 로봇에서 컴퓨팅 이중화(Compute Redundancy)는 컴퓨팅 구성요소를 사용할 수 없게 되거나 신뢰할 수 없는 결과를 생성하는 경우에도 지속적인 제어, 고장 격리(Fault Containment), 안전한 성능 저하(Safe Degradation)를 제공한다. 보행은 인지, 상태 추정, 경로 계획 및 결정론적 관절 제어의 협조에 의존하므로 하나의 컴퓨팅 장애가 전체 로봇에 영향을 줄 수 있다. 따라서 이중화는 단순히 프로세서를 복제하는 것이 아니라 핵심 컴퓨팅 기능을 보호하도록 설계해야 한다.

이중화 아키텍처(Redundancy Architecture)는 특정 고장이 발생했을 때 어떤 기능이 반드시 유지되어야 하는지를 식별하는 것에서 시작해야 한다. 비상 보호와 액추에이터 안전은 비핵심 검사 분석보다 더 높은 독립성을 요구하며, 고급 AI 기능을 사용할 수 없는 경우에도 기본적인 보행은 유지해야 할 수 있다. 이러한 기능 분류에 따라 복제, 대체 처리(Fallback Processing), 독립 모니터링 또는 제어된 종료를 적용할 위치를 결정한다.

계층형 사족보행 아키텍처(Hierarchical Quadruped Architecture)는 자연스럽게 여러 수준의 컴퓨팅 독립성을 제공한다. 엣지 GPU 서버(Edge GPU Server)는 고성능 AI 워크로드를 실행하고, 젯슨(Jetson)은 온보드 자율성을 유지하며, 실시간 제어기(Real-Time Controller)는 결정론적 운동 및 안전 기능을 보존할 수 있다. 따라서 하나의 계층에 장애가 발생하더라도 전체 시스템이 통제 불가능한 상태로 상실되는 것이 아니라 정의된 성능 저하 전략에 따라 사용 가능한 기능이 단계적으로 감소해야 한다.

이중화는 액티브-액티브(Active-Active), 액티브-스탠바이(Active-Standby) 또는 기능적 다양성 구성(Functionally Diverse Configuration)으로 구현할 수 있다. 액티브-액티브 노드는 기능을 동시에 실행하여 출력을 비교하거나 워크로드를 분산할 수 있다. 액티브-스탠바이 아키텍처는 주 노드 장애 이후 역할을 인계할 수 있는 보조 노드를 유지한다. 기능적 다양성은 서로 다른 컴퓨팅 경로나 알고리즘을 이용하여 하나의 공통 고장 메커니즘이 모든 채널을 동시에 무력화할 가능성을 줄인다.

완전한 하드웨어 복제(Complete Hardware Duplication)가 항상 필요하거나 바람직한 것은 아니다. 사족보행 로봇은 중량, 부피, 전력 소비, 방열, 배선 및 비용에 엄격한 제약을 가진다. 따라서 이중화는 선택적으로 적용해야 한다. 핵심 상태 추정, 명령 감독, 안전 모니터링 및 필수 내비게이션은 자원 복제가 필요할 수 있지만 대규모 VLM 추론이나 검사 분석은 로봇이 안전 상태에 도달하는 것을 방해하지 않는다면 일시적으로 제거할 수 있다.

젯슨 컴퓨팅 계층(Jetson Computing Layer)은 인지, 위치추정, 내비게이션, 임무 로직 및 하위 제어 계층과의 통신을 통합하므로 이중화의 중요한 대상이다. 보조 젯슨(Secondary Jetson) 또는 다른 독립 컴퓨팅 노드는 필수 서비스를 유지하거나 복제된 상태 정보를 수신할 수 있다. 백업 시스템의 목적이 저속 내비게이션, 제어된 후퇴 또는 안전한 임무 종료라면 반드시 주 시스템과 동일한 성능을 가질 필요는 없다.

엣지 GPU 서버(Edge GPU Server)는 일부 AI 서비스를 온보드 또는 외부에서 선택적으로 실행할 수 있을 때 또 다른 형태의 컴퓨팅 이중화를 제공한다. 고부하 추론은 일반적으로 서버에서 실행하면서 더 작은 대체 모델(Fallback Model)을 젯슨에 유지할 수 있다. 반대로 통신 조건이 허용된다면 서버 측 처리가 장애 또는 과부하 상태의 온보드 AI 서비스를 일시적으로 대체할 수 있다. 이러한 방식은 동일한 하드웨어의 단순 복제보다 워크로드 이식성(Workload Portability)과 고장 복구를 결합한다.

실시간 제어기(Real-Time Controller)는 상위 컴퓨팅 장애로부터 충분한 독립성을 유지해야 한다. 젯슨이 충돌하거나 응답하지 않더라도 관절 제어 루프, 액추에이터 제한, 워치독(Watchdog) 및 핵심 안전 기능은 사전에 정의된 안전 대응을 실행할 수 있을 만큼 지속되어야 한다. 이러한 분리를 통해 리눅스(Linux), ROS 2, GPU 소프트웨어 또는 AI 추론 장애가 결정론적 모터 제어 권한을 직접적으로 상실시키는 것을 방지할 수 있다.

상태 모니터링(Health Monitoring)은 컴퓨팅 노드를 실제로 사용할 수 있는지 판단하기 위해 필요하다. 하트비트 메시지(Heartbeat Message)는 통신이 완전히 상실된 상태를 감지할 수 있지만 잘못된 연산 결과를 식별하기에는 충분하지 않다. 따라서 태스크 실행 상태, 데이터 최신성, 타이밍 동작, 메모리 상태, 온도, 통신 상태, 동기화 상태 및 애플리케이션 수준 타당성을 함께 검사하여 온라인 상태이지만 더 이상 신뢰할 수 있는 결과를 제공하지 못하는 노드를 감지해야 한다.

워치독(Watchdog)은 프로세서 또는 소프트웨어 장애를 감지하기 위한 로컬 메커니즘을 제공한다. 독립적인 하드웨어 워치독은 핵심 실행 상태를 감시하고 예상된 활동이 사라지면 재시작 또는 격리를 수행할 수 있다. 소프트웨어 워치독은 개별 프로세스와 서비스를 추가로 감시할 수 있다. 이중화 아키텍처에서는 동일한 장애 프로세서가 핵심 기능을 수행하면서 동시에 해당 기능 자체가 정상적인지를 판단하는 구조를 피해야 한다.

페일오버(Failover)는 제어 권한이 이전되기 전에 신뢰할 수 있는 장애 감지를 필요로 한다. 주 컴퓨터와 백업 컴퓨터가 동시에 동일한 액추에이터 또는 임무 기능을 제어한다고 판단하면 서로 충돌하는 명령이 발생할 수 있다. 따라서 아키텍처에는 명확한 소유권(Ownership), 중재(Arbitration) 및 상태 전환 메커니즘이 필요하다. 제어 권한은 기동, 복구, 통신 손실 및 모호한 노드 상태에 대한 처리 규칙을 포함하는 결정론적 절차에 따라 노드 사이에서 이동해야 한다.

상태 동기화(State Synchronization)는 스탠바이 노드가 얼마나 빠르게 역할을 인계할 수 있는지를 결정한다. 백업 프로세서는 현재 로봇 자세, 지도 상태, 임무 진행 상태, 관절 상태, 캘리브레이션 정보 및 최근 센서 이력을 필요로 할 수 있다. 모든 내부 상태를 지속적으로 복제하면 비용이 증가하고 소프트웨어 결합도가 지나치게 높아질 수 있으므로 백업 시스템이 필수 운용을 안전하게 재개할 수 있는 최소 복구 상태(Minimal Recoverable State)를 정의해야 한다.

시간 동기화(Time Synchronization)는 서로 다른 노드의 출력을 공통 시간 기준에서 비교해야 하므로 이중화 연산의 기본 요소이다. PTP로 동기화된 클록은 두 개의 추정값이 동일한 물리적 시점에 대응하는지를 시스템이 판단할 수 있게 한다. 공통 시간이 없으면 측정 시점이나 네트워크 지연으로 발생한 차이가 컴퓨팅 결과의 불일치로 잘못 판단될 수 있으며, 이에 따라 고장 감지 및 투표(Voting)의 신뢰성이 저하될 수 있다.

데이터 경로 이중화(Data-Path Redundancy)도 고려해야 한다. 복제된 컴퓨터가 하나의 스위치, 전력 변환기 또는 통신 케이블을 공유한다면 여전히 단일 고장점(Single Point of Failure)을 가질 수 있다. 핵심 채널에는 독립적인 네트워크 경로, 분리된 스위치 포트, 이중화 전원 공급 또는 대체 통신 버스가 필요할 수 있다. 따라서 이중화 분석은 프로세서 개수만 계산하는 것이 아니라 전체 의존성 체인(Dependency Chain)을 검토해야 한다.

전원 아키텍처(Power Architecture)는 컴퓨팅 이중화와 밀접하게 연결된다. 동일한 보호되지 않은 변환기에서 전원을 공급받는 두 컴퓨터는 전원 이상 발생 시 동시에 장애가 발생할 수 있다. 핵심 이중화 노드는 독립적으로 보호되는 전원 분기 또는 적절히 분리된 DC-DC 변환 경로를 사용할 수 있다. 전원 정상 신호(Power-Good Signal), 저전압 모니터링, 제어된 기동 및 독립 종료 메커니즘은 컴퓨팅 장애와 근본적인 전기적 장애를 구분하는 데 도움이 된다.

열 설계(Thermal Design) 역시 공통 원인 고장(Common-Cause Failure)을 발생시킬 수 있다. 동일하게 냉각이 불충분한 인클로저에 두 개의 이중화 프로세서를 설치하면 높은 주변 온도에서 동시에 열 스로틀링(Thermal Throttling) 또는 종료가 발생할 수 있다. 따라서 물리적 배치, 열전달 경로, 공기 흐름 및 열 모니터링도 이중화 설계의 일부로 평가해야 한다. 두 채널이 동일한 부적절한 환경 조건에 의존한다면 논리적 복제만으로는 충분한 보호를 제공할 수 없다.

네트워크 분리(Network Partitioning)는 또 다른 고장 모드를 발생시킨다. 정상적인 컴퓨팅 노드라도 센서나 제어기로부터 네트워크상 격리되면 필요한 기능을 수행할 수 없다. 이중화 관리 시스템은 프로세서 장애와 통신 경로 장애를 구분하고 어떤 노드가 필요한 정보에 계속 접근할 수 있는지를 판단해야 한다. 필수 기능이 불필요한 하나의 통신 병목에 의존하지 않도록 네트워크 토폴로지와 서비스 배치를 설계해야 한다.

이중화 인지(Redundant Perception)는 반드시 동일한 센서나 동일한 알고리즘을 사용할 필요가 없다. 카메라, 라이다, IMU, 엔코더 및 발 힘 센서(Foot-Force Sensor)는 로봇 상태와 환경에 대해 서로 다른 관측 정보를 제공한다. 독립적인 센서 모달리티(Modality)를 교차 검증하면 단순한 프로세서 복제로는 식별하기 어려운 비정상적인 결과를 감지할 수 있다. 동일한 소프트웨어 결함이나 손상된 입력 데이터가 동일한 이중화 파이프라인에 같은 방식으로 영향을 줄 수 있는 경우 기능적 다양성이 특히 중요하다.

성능 저하 운용(Degraded Operation)은 이중화 전략의 일부로 명확하게 정의해야 한다. 엣지 GPU 서버가 상실되면 대규모 모델 추론 기능은 중단되더라도 정상적인 보행은 유지할 수 있다. 하나의 인지 서비스가 상실되면 속도를 낮추거나 이동 가능한 지형을 제한할 수 있다. 주 젯슨 컴퓨팅 장애는 백업 내비게이션 또는 제어된 정지를 실행할 수 있으며, 핵심 실시간 제어의 장애는 가능한 가장 안전한 물리적 상태로 즉시 전환해야 할 수 있다.

고장 이후 복구(Recovery)는 페일오버만큼 신중하게 제어해야 한다. 재부팅된 노드는 다시 통신하기 시작했다는 이유만으로 자동으로 제어 권한을 회복해서는 안 된다. 설정을 복원하고 시간을 동기화하며 센서와 네트워크 접근을 확인하고 필요한 상태를 복구한 뒤 정상적인 실행 상태를 검증해야 한다. 이러한 조건을 충족한 이후에만 이중화 관리자(Redundancy Manager)가 노드의 재통합 또는 제어 책임 이전을 허용해야 한다.

진단 및 이벤트 로깅(Diagnostics and Event Logging)은 현장 운용에서 이중화 동작을 이해하기 위해 필수적이다. 페일오버 원인, 노드 상태, 워치독 이벤트, 시간 오프셋, 통신 오류, 전원 조건, 열 상태, 제어 권한 전환 및 복구 동작을 타임스탬프와 함께 기록해야 한다. 이러한 기록을 통해 이중화 시스템이 실제 하드웨어 장애, 소프트웨어 결함, 네트워크 문제 또는 잘못된 상태 판단에 반응했는지를 엔지니어가 분석할 수 있다.

검증(Validation)에서는 이중화 하드웨어가 신뢰성을 자동으로 보장한다고 가정하지 않고 의도적으로 장애를 주입해야 한다. 프로세서 충돌, 애플리케이션 정지, 네트워크 단절, 메시지 지연, 손상된 데이터, PTP 동기화 상실, GPU 과부하, 열 스로틀링, 전원 차단 및 복구 조건을 시험해야 한다. 중요한 평가 대상은 단순히 백업 시스템의 존재 여부가 아니라 로봇이 장애를 감지하고 요구된 시간 안에 의도된 운용 상태로 전환하는지 여부이다.

궁극적으로 컴퓨팅 이중화(Compute Redundancy)는 사족보행 로봇을 위한 계층형 복원력 아키텍처(Layered Resilience Architecture)를 형성한다. 엣지 GPU 자원, 젯슨 컴퓨팅, 실시간 제어기, 통신 네트워크, 동기화된 시간, 전력 분배, 워치독 및 안전 메커니즘은 숨겨진 공통 단일 고장점을 만들지 않으면서 상호 협력해야 한다. 적절하게 설계된 이중화는 고급 AI 기능에 장애가 발생하더라도 점진적으로 성능을 저하시키면서 결정론적 제어와 가능한 가장 안전한 물리적 동작을 유지할 수 있도록 한다.
