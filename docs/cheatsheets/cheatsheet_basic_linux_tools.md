# Basic Linux Tools

| **Command**              | **Description**                      |
| ------------------------ | ------------------------------------ |
| **General**              |                                      |
| `sudo openvpn user.ovpn` | Connect to VPN                       |
| `ifconfig`/`ip a`        | Show our IP address                  |
| `netstat -rn`            | Show networks accessible via the VPN |
| `ssh user@10.10.10.10`   | SSH to a remote server               |
| `ftp 10.129.42.253`      | FTP to a remote server               |
| **tmux**                 |                                      |
| `tmux`                   | Start tmux                           |
| ++ctrl+b++ ++ctrl+c++    | tmux: new window                     |
| ++ctrl+b++ ++1++         | tmux: switch to window (`1`)         |
| ++ctrl+"%"++             | tmux: split pane vertically          |
| ++ctrl+"\""++            | tmux: split pane horizontally        |
| ++ctrl+right++           | tmux: switch to the right pane       |
| **Vim**                  |                                      |
| `vim file`               | vim: open `file` with vim            |
| `esc+i`                  | vim: enter `insert` mode             |
| `esc`                    | vim: back to `normal` mode           |
| `x`                      | vim: Cut character                   |
| `dw`                     | vim: Cut word                        |
| `dd`                     | vim: Cut full line                   |
| `yw`                     | vim: Copy word                       |
| `yy`                     | vim: Copy full line                  |
| `p`                      | vim: Paste                           |
| `:1`                     | vim: Go to line number 1.            |
| `:w`                     | vim: Write the file 'i.e. save'      |
| `:q`                     | vim: Quit                            |
| `:q!`                    | vim: Quit without saving             |
| `:wq`                    | vim: Write and quit                  |

#  
