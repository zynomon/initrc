initRC 
is more leaner than other init system well it's just a concept for now. i dont have enough time these days to manage to make it real

flowchart TD
    A[PID 1: initrcd] --> B
    
    subgraph "Parallel Group 1 (Immediate)"
        B[network.rc]
        C[syslog.rc]
        D[udev.rc]
    end
    
    subgraph "Parallel Group 2 (After Parralel group one ends)"
        E[nginx.rc]
        F[sshd.rc]
    end
    
    subgraph "Serial Group (--unp services unparalleled that means no system break)"
        G[apt.rc --unp]
        H[mysql.rc --unp]
    end
    
    B --> E
    B --> F
    G -.->|"waits for lock"| H

    
