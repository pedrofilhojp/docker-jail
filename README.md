# Disciplina Virtualização [ Compreendendo as tecnologias de containers ]

- Curso Superior em Redes de Computadores
- Disciplina de Virtualização
- Prof. Pedro Filho (pedro.carvalho@ifpb.edu.br)

## Objetivo
Segundo o prof. Carlos Mazieiro, em seu [livro de Sistemas Operacionais](https://wiki.inf.ufpr.br/maziero/doku.php?id=socm:start)
, no capítulo 32.1 na qual aborda os critérios de classificação de tipos de máquinas virtuais, temos as "Máquinas virtuais de sistemas operacionais". 

Esse tipo de VM é contruída para suportar espaços de usuários distintos sobre o mesmo sistema operacional, permitindo que cada ambiente virtual possua seus próprios recursos lógicos, como espaço de armazenamento, mecanismos de IPC e interfaces de rede. Sob essa classificação pode-se citar exemplos de sistemas, o Docker, Solaris Containers e FreeBSD jail.

Portanto, este repositório tem objetivo de servir de guia prático de estudo diante dos conceitos apresentados em sala de aula com relação as tecnologias por trás de um container Docker.

Aqui, utilizaremos consultas no docker-daemon via API Rest e implementação de Jail em sistemas linux. Além disso, os alunos serão desafiados a implementar projetos que poderão ser utilizados em ambiente real, principalmente na área de Devops.

