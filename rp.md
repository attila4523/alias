# Random Password

## pwgen

```bash
alias rp='pwgen -1ysc 32 2'
```

## urandom

```bash
function rp(){
    MAX=10;
    if [[ ! -z "$1" ]]; then MAX=$1; fi
	for i in $(seq 1 1 $MAX); do LC_CTYPE=C tr -dc A-Za-z0-9 < /dev/urandom | fold -w 32 | head -n 1; done
}