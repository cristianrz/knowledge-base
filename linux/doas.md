# doas

## Basic `doas.conf`

```
permit :wheel
```

## No password

```
permit nopass :wheel
```

## Cache passwords

```
permit persist :wheel
```

## Allow specific command

```conf
permit nopass :upgraders as root cmd /bin/apt args upgrade
```
