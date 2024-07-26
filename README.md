# Chuleta Laboratorio ACL

## Configurar Router

```bash
conf term
interface (interface)
ip address (direccion_ip) (mascara)
no shutdown
```

## Configurar Enrutamiento

```bash
router rip
network (ip_red)
ip route (ip_red) (mascara) (ip_salto)
```

## ACL

numero = 101, 102, etc.

propiedad_permitida = permit, deny, any.

protocolo = ip, tcp, icmp.

ip_red = 10.10.10.0, etc.

mascara_wildcard = 0.0.0.255, etc.

acceso = in, out.

```bash
access-list (numero) (propiedad_permitida) (protocolo) (ip_red) (mascara_wildcard) (regla)
interface (interface)
ip access-group (numero) (acceso)
```

- Ejemplo 1: Proteger Red Empresarial

```bash
access-list 101 permit ip 10.10.10.0 0.0.0.255 any
access-list 101 deny ip any any
interface (interface)
ip access-group 101 in
```

La primera línea definida en la lista de acceso “101” sólo permite que los usuarios empresariales válidos de la red 10.10.10.0 accedan al router. La segunda línea no se requiere realmente debido al deny all implícito, pero se agregó para facilitar la comprensión.

```bash
access-list 102 permit tcp any any established
access-list 102 permit icmp any any echo-reply
access-list 102 permit icmp any any unreachable
access-list 102 deny ip any any
interface (interface)
ip access-group 102 out
```

La palabra clave established en esta línea sólo permite el tráfico TCP que se origina en la red 10.10.10.0.

La segunda línea sólo permite que los pings exitosos vuelvan a la red empresarial. La tercera línea permite mostrar los mensajes de los ping que no fueron exitosos.

- Ejemplo 2: Proteger Red DMZ

```bash
access-list 111 permit ip 10.1.1.0 0.0.0.255 any
access-list 111 deny ip any any
interface (interface)
ip access-group 111 in
```

```bash
access-list 112 permit tcp any host 10.1.1.10 eq www
```

Esta línea permite que los servicios de la World Wide Web destinados al servidor de Web entren a la red DMZ.

```bash
access-list 112 permit icmp 10.10.10.0 0.0.0.255 host 10.1.1.10
```

Esta línea sólo permite que los hosts de la red empresarial hagan ping al servidor de Web.


```bash
access-list 112 deny ip any any
interface (interface)
ip access-group 112 out
```