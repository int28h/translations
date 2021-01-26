# Процессы и потоки  
  
[Сурс](https://docs.oracle.com/javase/tutorial/essential/concurrency/procthread.html)  
  
В параллельном программировании есть две базовые единицы исполнения: процесс (process) и поток (thread). В Java параллельное программирование завязано в основном на потоки, но процессы также важны.  
  
Как правило, компьютерная система имеет много активных процессов и потоков. Это также является истиной и для систем с одним ядром выполнения - т.е. для систем, в которых в единицу времени выполняется только один поток. Таким образом, время обработки порционно делится среди всех процессов и потоков с помощью функции ОС, называемой квантованием времени (time slicing).  
  
Становится всё более и более распространённым существование компьютерных систем с несколькими процессорами или процессорами с несколькими ядрами исполнения. Это значительно увеличивает возможности системы параллельно выполнять процессы и потоки — но параллелизм возможен даже в простых системах, не имеющих нескольких процессоров или ядер исполнения.  
  
## Процессы  
  
A process has a self-contained execution environment. A process generally has a complete, private set of basic run-time resources; in particular, each process has its own memory space.  
  
Processes are often seen as synonymous with programs or applications. However, what the user sees as a single application may in fact be a set of cooperating processes. To facilitate communication between processes, most operating systems support Inter Process Communication (IPC) resources, such as pipes and sockets. IPC is used not just for communication between processes on the same system, but processes on different systems.  
  
Most implementations of the Java virtual machine run as a single process. A Java application can create additional processes using a ProcessBuilder object. Multiprocess applications are beyond the scope of this lesson.  
  
## Потоки  
  
Threads are sometimes called lightweight processes. Both processes and threads provide an execution environment, but creating a new thread requires fewer resources than creating a new process.  
  
Threads exist within a process — every process has at least one. Threads share the process's resources, including memory and open files. This makes for efficient, but potentially problematic, communication.  
  
Multithreaded execution is an essential feature of the Java platform. Every application has at least one thread — or several, if you count "system" threads that do things like memory management and signal handling. But from the application programmer's point of view, you start with just one thread, called the main thread. This thread has the ability to create additional threads, as we'll demonstrate in the next section.  
  