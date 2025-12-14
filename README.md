initRC 
is more leaner than other init system well it's just a concept for now. i dont have enough time these days to manage and make it real.

the core idea is to use those extra 50% of unused cpu pressure at work and make the device more faster 
initRC will run different command in different virtual terminals if one command finishes it will run another making the bootup speed faster unlike the traditional 1 command at a time method 
it could run all services ( 30+ ) at once but we will limit it to 10 slots. but user could control how many they want to run at once and after graphical session turns on it will has another type of limiter which would be disabled by default but user could enable how many daemons to run simultaneously.

exceptional command line arguments.
--unp ( unparallel ) tag : its a new command line argument that ensures that a service doesn't runs simultaneously with other service
--switch above/below to control the position to run it.
( and other ordinary init command line arguments )

```mermaid
flowchart TD
    A["Boot Start:<br/>20 terminal slots available"] --> W1
    
    subgraph W1["Wave 1:<br/>Fill all 20 slots"]
        direction LR
        T1["Terminal 1<br/>mount"]
        T2["Terminal 2<br/>mknod"]
        T3["Terminal 3<br/>swapon"]
        T4["Terminal 4<br/>dhclient"]
        T20["Terminal 20<br/>..."]
    end
    
    W1 --> B{"Wait for ANY terminal<br/>to finish"}
    
    B --> C["Slot freed!<br/>Start next service"]
    
    C --> D{"More --unp services?"}
    
    D -- "No" --> W2["Wave 2:<br/>Normal parallel services"]
    D -- "Yes" --> E["Queue for --unp lock"]
    
    E --> F["--unp service runs<br/>(takes 1 slot exclusively)"]
    
    F --> W2
    
    subgraph W2["Wave 2:<br/>Continue filling 20 slots"]
        direction LR
        U1["Terminal 1<br/>udev"]
        U2["Terminal 2<br/>fsck"]
        U3["Terminal 3<br/>dbus"]
        U4["Terminal 4<br/>..."]
        U20["Terminal 20<br/>getty"]
    end
```
Making the boot very faster..
