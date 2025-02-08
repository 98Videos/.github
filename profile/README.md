# Projeto 98Videos - Hackaton FIAP 2025
Este é um projeto desenvolvido para o Hackaton curso de Software Architecture da FIAP em 2024. A proposta é desenvolver um sistema aplicando tecnologias, metodologias e boas práticas aprendidas ao longo do curso.
______________________________
# O 98Videos
O sistema 98Videos é um sistema de extração de imagens de videos enviado pelo usuário, que permite que ele realiza o download dessas imagens em um arquivo compacto no formato ZIP.
Suas funcionalidades são:
 - **Upload do video:** O sistema permite o upload do video para extração das imagens
 - **Download das imagens:** O sistema permite o download das imagens após o processamento
 - **Envio de e-mail:** O sistema envia o e-mail para quando o processamento foi concluido ou quando ocorre falha
______________________________
# Integrantes do grupo
- André Torres Corrêa
- Angelo Costa Neto
- Mauro Tuyoshi Imamura Júnior
______________________________
# Arquitetura 
O sistema foi desenvolvido com uma arquitetura de microsserviços, implantada em um cluster EKS (Kubernetes) na AWS. A comunicação entre os serviços é feita de forma assíncrona através de mensageria, utilizando o Amazon SQS. O serviço media-management-api armazena o status dos vídeos em um banco de dados PostgreSQL e realiza o upload dos vídeos para um bucket S3, configurado exclusivamente para armazenar vídeos.

O video-processor-worker utiliza o SQS para obter informações sobre os vídeos a serem processados, acessando-os no bucket S3 da AWS. O processamento dos vídeos é feito com a biblioteca FFmpeg, e, ao finalizar, o video-processor faz o upload de um arquivo ZIP contendo as imagens geradas para outro bucket S3, dedicado a armazenar arquivos processados. Após o processamento, o video-worker notifica a media-management-api via HTTP, informando o término do processo. Em caso de falha, também envia uma notificação via HTTP para a media-management-api.

A media-management-api, ao receber a atualização de status do processamento, notifica o usuário via email, seja no caso de sucesso ou falha, utilizando um servidor SMTP (Mailtrap). O sistema de registro e autenticação de usuários é realizado pelo Cognito, que valida os tokens JWT. A media-management-api verifica o token JWT no Cognito para garantir a autenticidade das requisições.

Todo o provisionamento da infraestrutura é automatizado e gerenciado via Terraform, com repositórios dedicados para Infraestrutura como Código (IaC), garantindo uma gestão eficiente e escalável da infraestrutura. 

![VideosFIAP-Proposta2](https://github.com/user-attachments/assets/3889ee62-cc11-4376-ba90-da148b0d3c2d)
____________________________
# Serviços
- media-management-api
- video-processor-worker
____________________________
# Autenticação
A autenticação é feita pelo serviço padrão do cognito
____________________________
# Infraestrutura
O provisionamento de infraestrutura é feito via Terraform nos seguintes repositórios:
- infrastructure
____________________________
# Stack
- .Net 8
- PostgreSQL
- Kubernetes
- SQS
- MassTransit
- S3
- XUnit
- lib FFMPEG
- NUnit
- Moq
- Entity Framework
- Cognito
- SMTP Server
