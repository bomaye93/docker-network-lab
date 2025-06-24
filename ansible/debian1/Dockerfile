FROM debian

# Installation des paquets nécessaires
RUN apt update && apt install -y \
    openssh-server \
    sudo \
    curl \
    dnsutils \
    python3 \
    python3-pip

# Création d’un utilisateur "ansible" avec mot de passe "ansible"
RUN useradd -m ansible && echo "ansible:ansible" | chpasswd && \
    echo "ansible ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# Préparation du service SSH
RUN mkdir /var/run/sshd

# Exposition du port SSH
EXPOSE 22

# Démarrage du service SSH
CMD ["/usr/sbin/sshd", "-D"]
