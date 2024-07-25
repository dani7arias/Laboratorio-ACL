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

## Proteger red

### Proteger red empresarial:

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

Ejemplo:

```bash
access-list 101 permit ip 10.10.10.0 0.0.0.255 any
access-list 101 deny ip any any
interface (interface)
ip access-group 101 in
```
