```mermaid
flowchart TD
    subgraph Clients["🌐 Client & Admin Layer"]
        Browser["User / Web Browser"]
        SysAdmin["SysAdmin (SSH & WinSCP)"]
    end

    subgraph ProxmoxHost["🖥️ Proxmox VE Host (Virtualization Engine)"]
        
        subgraph NAT["Port Forwarding & Routing"]
            P80["Port 80 (HTTP Traffic)"]
            P2223["Port 2223 (Secure SSH/SFTP)"]
        end

        subgraph LXC["📦 LXC Container ID 100 (Ubuntu OS)"]
            SSHD["SSH Daemon (Port 22)"]
            
            subgraph Web["Web & Reverse Proxy Layer"]
                Nginx["Nginx Web Server"]
            end

            subgraph App["Application Stack"]
                PHPFPM["PHP 8.1 FPM"]
                Laravel["Laravel 10 / Node.js App"]
            end

            subgraph DB["Data Layer"]
                MariaDB[("MariaDB Database")]
            end
        end
    end

    Browser -->|HTTP Request| P80
    SysAdmin -->|Management Access| P2223

    P80 --> Nginx
    P2223 --> SSHD

    Nginx -->|FastCGI Pass| PHPFPM
    PHPFPM -->|Executes Code| Laravel
    Laravel -->|Database Query| MariaDB

    style ProxmoxHost fill:#1e1e2e,stroke:#89b4fa,stroke-width:2px,color:#cdd6f4
    style LXC fill:#313244,stroke:#a6e3a1,stroke-width:2px,color:#cdd6f4
