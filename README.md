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
    A["Boot Start<br>10 terminal slots available"] --> W1
    
    subgraph W1["Wave 1: Initial Services"]
        direction LR
        T1["T1: mount"]
        T2["T2: mknod"]
        T3["T3: swapon"]
        T4["T4: dhclient"]
        T5["T5: kmod"]
        T6["T6: network"]
        T7["T7: syslog"]
        T8["T8: random"]
        T9["T9: urandom"]
        T10["T10: ..."]
    end
    
    W1 --> B{"Wait for ANY<br>terminal to finish"}
    
    B --> C["Slot freed!<br>Start next service"]
    
    C --> D{"More<br>--unp services?"}
    
    D -- "No" --> W2
    D -- "Yes" --> E["Queue for<br>--unp lock"]
    
    E --> F["--unp service runs<br>(exclusive slot)"]
    
    F --> W2
    
    subgraph W2["Wave 2: Parallel Services"]
        direction LR
        U1["T1: udev"]
        U2["T2: fsck"]
        U3["T3: dbus"]
        U4["T4: cron"]
        U5["T5: sshd"]
        U6["T6: ntp"]
        U7["T7: docker"]
        U8["T8: nginx"]
        U9["T9: mysql"]
        U10["T10: getty"]
    end
    
    W2 --> G["Boot Complete"]
```
Making the boot very faster..
instead of running one service at a time
