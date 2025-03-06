# Vagrant
Utilizamos vagrant para crear nuestras box y utilizarlas posteriormente en un vagrantfile para desplegar el entorno de forma rapida y fiable
## Instalar Vagrant

Para instalar vagrant nos dirigimos a [vagrantup.com](https://www.vagrantup.com/) y nos dirigimos a la parte de Download


![Figura 1: Instalar Vagrant para Windows](img/1.%20decarga_vagrant.png)
*Instalar Vagrant en Windows*

## Crear box Ubuntu-Desk
Para preparar una box lo primero que tenemos que hacer es crear una maquina con el SO que queramos y configurala para vagrant, una vez tengamos Ubuntu-Desk instalado haremos lo siguiente:

1. Crear el usuario Vagrant con el siguiente comando:
    - Ejecuta lo siguiente:

        
            sudo adduser vagrant
        

    - Agrega el usuario vagrant al grupo sudo:

        
            sudo usermod -aG sudo vagrant

        ![Agregar usuario a sudo](img/2.%20agregar_sudo.png)
        

- Configurar sudo sin contraseña:
    - Ejecuta:

        
            echo "vagrant ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/vagrant

        ![Inicio sin contraseña](img/5.%20sudo_tee.png)
        

- Configurar SSH para Vagrant:
    - Crear carpeta ```.ssh``` en el home del usuario

        
            sudo mkdir -p /home/vagrant/.ssh

        ![carpeta .ssh](img/3.%20mkdir.ssh.png)
        

    - Descarga la clave pública oficial de Vagrant:

        
            sudo curl -fsSL https://raw.githubusercontent.com/hashicorp/vagrant/master/keys/vagrant.pub -o /home/vagrant/.ssh/authorized_keys
        
    
    - Ajusta los permisos:

        
            sudo chmod 600 /home/vagrant/.ssh/authorized_keys
            sudo chmod 700 /home/vagrant/.ssh
            sudo chown -R vagrant:vagrant /home/vagrant/.ssh

        ![Ajustar permisos](img/4.%20permisos_ssh.png)
        
   
    - Verifica que el servicio SSH está activo:

            sudo systemctl enable ssh
            sudo systemctl start ssh
        
- Instalar VirtualBox Guest Additions:

    - Ejecutamos lo siguiente:
        
            sudo apt install -y build-essential dkms linux-headers-$(uname -r)
            sudo mount /dev/cdrom /mnt
            cd /mnt
            sudo ./VBoxLinuxAdditions.run

        ![Guest Additions](img/6.%20Guest_additions.png)
    
    - Reinicia la MV:

            sudo reboot

- Limpiar la Máquina antes de crear la box:

    - Para reducir el tamaño de la box:

            sudo apt autoremove -y
            sudo apt clean
            sudo rm -rf /var/cache/apt/archives/*
            sudo rm -rf /tmp/*

    - Eliminar el historial de comandos:

            cat /dev/null > ~/.bash_history && history -c

    - Apagar la MV:

            sudo shutdown -h now

- Empaquetar la Box:

    - Desde el anfitrión:

            vagrant package --base NOMBRE_DE_LA_VM --output ubuntu24.04.box

        ![Empaquetar la máquina en la box](img/7.%20Empaquetar_box.png)

- Añadir la Box a Vagrant:

    - Ejecuta desde el directorio donde empaquetaste la Box:

            vagrant box add ubuntu-desktop ubuntu-desktop.box

        ![Añadir Box a vagrant](img/8.%20añadir_box.png)

    - Verifica que se ha añadido a la lista de box de Vagrant:

            vagrant box list





    











    



