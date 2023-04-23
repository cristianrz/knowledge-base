# Lynis tests

$$
\text{Score} = \text{Index} / \text{Recommendations} / \text{Red} * 1000
$$

| Distro                        | Index | Recommendations | Red lines | Kernel ok | Kernel fail | Score |
| ----------------------------- | ----- | --------------- | --------- | --------- | ----------- | ----- |
| Mac                           | 71    | 15              | 9         |           |             | 530   |
| OpenBSD                       | 69    | 23 + 4          | 14        |           |             | 180   |
| Alpine                        | 61    | 28              | 30        | 73        | 91          | 73    |
| Red Hat CIS lvl 1             | 75    | 30 + 1          | 15        | 70        | 94          | 161   |
| Alma + CIS lvl 1              | 75    | 32              | 18        | 73        | 91          | 130   |
| Debian minimal                | 65    | 32              | 41        | 74        | 90          | 50    |
| Arch Hardened                 | 62    | 34 + 2          | 39        | 105       | 59          | 44    |
| Whonix                        | 70    | 34 + 1          | 36        | 76        | 88          | 56    |
| Alma Linux                    | 64    | 40              | 20        |           |             | 80    |
| OpenSUSE leap                 | 65    | 41 + 1          | 65        | 53        | 111         | 24    |
| Void                          | 60    | 42              | 30        |           |             | 48    |
| Ubuntu                        | 57    | 41 + 4          | 37        | 68        | 96          | 34    |
| Fedora                        | 65    | 42              | 73        |           |             | 21    |
| Debian default, Lynis updated | 62    | 49              | 62        |           |             | 20    |
| Debian mine                   | 64    | 49              | 68        |           |             | 19    |
| Rocky                         | 64    | 52              | 21        |           |             | 59    |
| Pentoo                        |       |                 |           | 107       | 57          |       |


## Top 3 server

1. Alma CIS
2. RedHat CIS
2. OpenBSD

## Top 3 desktop

1. Arch hardened
2. Fedora
3. Whonix