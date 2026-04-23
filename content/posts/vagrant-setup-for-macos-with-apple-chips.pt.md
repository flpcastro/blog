---
title: "Configurando o Vagrant no macOS com chips Apple"
date: 2026-03-16
draft: false
tags: ["vagrant", "macos", "apple"]
categories: ["vagrant"]
---

![Vagrant](/images/vagrant.webp)


E aí, pessoal!

Neste post curto, vamos configurar um ambiente simples para criar VMs no macOS com chips Apple (M1/M2/M3 ou mais novos) usando o Vagrant.

## 1. Introdução
### O que é o Vagrant?

O Vagrant é uma ferramenta que nos permite criar e gerenciar máquinas virtuais de forma fácil e eficaz. Com ele, podemos criar VMs a partir de um arquivo de configuração chamado Vagrantfile, o que facilita o compartilhamento das configurações da máquina e mantém a consistência entre os ambientes. Esse software usa por baixo dos panos os principais provedores de virtualização, como VirtualBox, VMware, QEMU, Parallels, Docker, Hyper-V, entre outros.

### Principais funcionalidades
- Ambientes consistentes e reproduzíveis: através do arquivo Vagrantfile, conseguimos usar o mesmo ambiente para todas as pessoas desenvolvedoras envolvidas em um projeto, por exemplo.

- Redução de trabalho manual e de erros: como as configurações do ambiente são feitas por código, o trabalho manual para criar várias VMs diminui drasticamente, reduzindo também a margem para erros.

## 2. Preparando o ambiente
### Requisitos
- **SO**: macOS 11.0 (Big Sur), mas é recomendado usar a versão mais recente do macOS (como Monterey 12.0 ou Ventura 13.0) para melhor compatibilidade e performance.
- **Processador**: Apple M1/M2/M3 ou mais novo...
- **Gerenciador de pacotes**: [Homebrew](https://brew.sh/)

### Instalação
Primeiro, vamos instalar o Homebrew, caso você ainda não tenha.
```bash
# Instalar o Homebrew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Agora, vamos instalar o [QEMU](https://www.qemu.org/), um emulador e virtualizador de máquinas virtuais de código aberto. Você pode escolher outro se preferir, como VMware, VirtualBox...
```bash
# Instalar o QEMU
brew install qemu
```

Por fim, vamos baixar o [Vagrant](https://developer.hashicorp.com/vagrant) em si e o plugin do **QEMU**.
```bash
# Instalar o Vagrant
brew tap hashicorp/tap
brew install hashicorp/tap/hashicorp-vagrant

# Instalar o plugin do Vagrant para o QEMU
vagrant plugin install vagrant-qemu
```

## 3. Configuração da VM (arm64)
Crie uma pasta para armazenar o arquivo de configuração do Vagrant.
```bash
# Criar um diretório
mkdir linux-machine && cd linux-machine
```

Dentro do diretório criado, gere o arquivo de configuração da máquina.
```bash
# Gerar um Vagrantfile
touch Vagrantfile
```

Usando um editor de texto de sua preferência (nano, vim, VSCode...), adicione o seguinte código:
```bash
Vagrant.configure("2") do |config|
  config.vm.box = "perk/ubuntu-2204-arm64"
  config.vm.provider "qemu" do |qemu|
    # defina a porta de sua preferência
    qemu.ssh_port = "9999"
  end
end
```
> **NOTA**: A imagem utilizada foi `perk/ubuntu-2204-arm64`. Mas você pode usar qualquer outra que seja compatível com o provider QEMU e com a arquitetura arm64. Você encontra mais imagens no [Vagrant Cloud](https://portal.cloud.hashicorp.com/vagrant/discover?architectures=arm64&providers=qemu). Por exemplo, a imagem `generic/debian12` também oferece compatibilidade com QEMU e arm64.

## 4. Subindo a VM
Baixe a imagem e inicie a VM.

```bash
# Baixar a imagem e subir
# Isso pode levar um tempo
vagrant up --provider=qemu
```

Acesse a linha de comando da máquina via SSH.
```bash
vagrant ssh
```

`Muito bem, seu novo ambiente está pronto. 🙃`

Aqui vão alguns **comandos** do Vagrant para te ajudar:
```bash
# Ver o status da máquina
vagrant status

# Ver imagens baixadas (box)
vagrant box list

# Remover imagens (box)
vagrant box remove nome-da-box
```
