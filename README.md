## Implementação de um cluster com Docker Swarm

### Instruções

1. Baixe a imagem no repositório do [Vagrant](https://app.vagrantup.com/debian/boxes/buster64/versions/10.20220912.1/providers/virtualbox.box)

2. Execute no terminal (aguarde terminar todo o processo)

```bash
vagrant up
```

3. Após a conclusão, veja o estado das máquians com o comando
```bash
vagrant status
```

4. Para acessar qualquer máquina, use o comando

```bash
vagrant ssh <hostname|ip>
```

5. Para parar todas as máquinas

```bash
vagrant halt
```

6. Para reiniciar as maquinas

```bash
vagrant reload
```

7. Para destruir o ambiente

```bash
vagrant destroy
```

### Gerenciado o cluster

1. Adionar um nó ao cluster (a partir do host `master`)

```bash
docker swarm init --advertise-addr <swarm master IP>
```

Resultado esperado:

```
Swarm initialized: current node (9ie9s0or5b7ihnnomcr4z3ht2) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-3r34h7bq66tatyoec3vlxa754qkfpuppyqqyw6sorrr4gk3d9n-51di89rzil9pqug9u311ys4kv 192.168.56.10:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```

2. Adicionar `worker` a partir de um token (a partir do `node` que será gerenciado)
```bash
docker swarm join <--token> <master IP>:2377
```
Resultado esperado:

```
This node joined a swarm as a worker.
```

3. Verifique se o `node` foi adicionado corretamente (a partir do host `master`)

```bash
docker node ls
```

Resultado esperado:

```
ID                            HOSTNAME   STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
9ie9s0or5b7ihnnomcr4z3ht2 *   master     Ready     Active         Leader           20.10.18
nr3tizdq2rrbahn7tk9ddl1la     node01     Ready     Active                          20.10.18
```