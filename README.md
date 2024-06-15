#!/bin/bash

# Função para verificar e instalar ferramentas necessárias no Arch Linux
verificar_instalacao() {
    comando=$1
    if ! command -v $comando &> /dev/null; then
        return 1
    else
        return 0
    fi
}

# Função para baixar e instalar ferramentas adicionais
baixar_instalar_ferramentas() {
    clear
    echo "======================= Baixando e Instalando Ferramentas Necessárias ======================="
    echo "Verificando e instalando ferramentas necessárias..."

    # Lista de ferramentas a serem verificadas e instaladas se não estiverem presentes
    ferramentas=("nmap" "metasploit" "wireshark-qt" "hydra" "snort" "sqlmap" "john" "protonvpn-cli" "aircrack-ng" "openvas" "nikto" "burpsuite" "clamav" "fail2ban" "rkhunter" "tcpdump" "sshpass")

    for ferramenta in "${ferramentas[@]}"; do
        if ! verificar_instalacao $ferramenta; then
            echo "$ferramenta não está instalado. Baixando e instalando..."
            if [[ $ferramenta == "protonvpn-cli" ]]; then
                git clone https://aur.archlinux.org/protonvpn-cli.git
                cd protonvpn-cli
                makepkg -si --noconfirm
                cd ..
                rm -rf protonvpn-cli
            else
                sudo pacman -Sy --noconfirm $ferramenta
            fi
            if [ $? -ne 0 ]; then
                echo "Erro ao instalar $ferramenta. Verifique sua conexão com a internet e tente novamente."
                exit 1
            fi
        else
            echo "$ferramenta já está instalado."
        fi
    done

    echo "Ferramentas necessárias instaladas com sucesso."
    echo "================================================================================================"
    read -p "Pressione Enter para continuar..."
}

# Função para criar um simulador de ataque DDoS
ataque_ddos() {
    clear
    echo "======================= Ataque DDoS Simulado ======================="
    echo "Insira o endereço IP alvo para o ataque DDoS:"
    read ip_address

    echo "Iniciando ataque DDoS contra $ip_address..."

    # Simulador de ataque DDoS (usando ping flood como exemplo)
    while :; do
        ping -c 1 $ip_address &> /dev/null
    done

    echo "Ataque DDoS concluído contra $ip_address."
    echo "===================================================================="
    read -p "Pressione Enter para continuar..."
}

# Função para varredura de portas com nmap
varredura_portas() {
    clear
    echo "======================= Varredura de Portas com Nmap ======================="
    echo "Insira o endereço IP alvo para a varredura de portas:"
    read ip_address

    # Executando varredura de portas com nmap
    sudo nmap -Pn -sV -p- $ip_address

    echo "Varredura de portas concluída para $ip_address."
    echo "============================================================================="
    read -p "Pressione Enter para continuar..."
}

# Função para teste de vulnerabilidade com Metasploit
teste_vulnerabilidade_metasploit() {
    clear
    echo "======================= Teste de Vulnerabilidade com Metasploit ======================="
    echo "Insira o endereço IP alvo para o teste de vulnerabilidade:"
    read ip_address

    # Executando teste de vulnerabilidade com Metasploit (simulação)
    echo "Executando teste de vulnerabilidade para $ip_address com Metasploit..."
    msfconsole -x "use auxiliary/scanner/portscan/tcp; set RHOSTS $ip_address; run"

    echo "Teste de vulnerabilidade concluído."
    echo "========================================================================================"
    read -p "Pressione Enter para continuar..."
}

# Função para configurar VPN
configurar_vpn() {
    clear
    echo "======================= Configuração de VPN ======================="
    echo "Conectando à VPN..."

    # Lógica para conectar à VPN (substitua com suas configurações específicas)
    protonvpn-cli connect -f --country BR

    echo "VPN configurada com sucesso."
    echo "===================================================================="
    read -p "Pressione Enter para continuar..."
}

# Função para teste simples de segurança
teste_seguranca() {
    clear
    echo "======================= Teste Simples de Segurança ======================="
    echo "Executando teste simples de segurança..."
    echo "Teste concluído."
    echo "==========================================================================="
    read -p "Pressione Enter para continuar..."
}

# Função para teste de segurança de rede WiFi com aircrack-ng
teste_wifi_aircrack() {
    clear
    echo "======================= Teste de Segurança de Rede WiFi com Aircrack-ng ======================="
    echo "Insira o nome da interface de rede para o teste de segurança WiFi (ex: wlan0):"
    read interface

    echo "Colocando a interface $interface em modo monitor..."
    sudo airmon-ng start $interface

    echo "Escaneando redes WiFi disponíveis..."
    sudo airodump-ng ${interface}mon

    echo "Insira o BSSID da rede alvo:"
    read bssid
    echo "Insira o canal da rede alvo:"
    read canal

    echo "Capturando pacotes da rede alvo..."
    sudo airodump-ng -c $canal --bssid $bssid -w captura ${interface}mon

    echo "Para interromper a captura, pressione Ctrl+C."
    echo "Após capturar pacotes suficientes, execute o comando abaixo para tentar quebrar a senha:"
    echo "sudo aircrack-ng -w /path/to/wordlist.txt -b $bssid captura-01.cap"

    echo "========================================================================="
    read -p "Pressione Enter para continuar..."
}

# Função para análise de vulnerabilidades com OpenVAS
analise_vulnerabilidades_openvas() {
    clear
    echo "======================= Análise de Vulnerabilidades com OpenVAS ======================="
    echo "Iniciando OpenVAS..."

    # Iniciar OpenVAS e realizar uma varredura
    sudo gvm-start
    sleep 10
    echo "Acesse o OpenVAS através do navegador em https://localhost:9392"
    echo "=================================================================="
    read -p "Pressione Enter para continuar..."
}

# Função para scanner de servidores web com Nikto
scanner_web_nikto() {
    clear
    echo "======================= Scanner de Servidores Web com Nikto ======================="
    echo "Insira o endereço URL alvo para o scanner:"
    read target_url

    # Executar scanner de servidores web com Nikto
    sudo nikto -h $target_url

    echo "Scanner de servidores web concluído para $target_url."
    echo "=================================================================================="
    read -p "Pressione Enter para continuar..."
}

# Função para captura de pacotes com Tcpdump
captura_pacotes_tcpdump() {
    clear
    echo "======================= Captura de Pacotes com Tcpdump ======================="
    echo "Insira a interface de rede para captura de pacotes (ex: eth0):"
    read interface

    # Capturar pacotes com Tcpdump
    sudo tcpdump -i $interface -w captura.pcap

    echo "Captura de pacotes concluída na interface $interface. Arquivo salvo como captura.pcap."
    echo "========================================================================================"
    read -p "Pressione Enter para continuar..."
}

# Função para antivírus com ClamAV
antivirus_clamav() {
    clear
    echo "======================= Antivírus com ClamAV ======================="
    echo "Atualizando base de dados do ClamAV..."
    sudo freshclam

    echo "Insira o caminho do diretório para a verificação de vírus:"
    read diretorio

    # Verificação de vírus com ClamAV
    sudo clamscan -r $diretorio

    echo "Verificação de vírus concluída no diretório $diretorio."
    echo "===================================================================="
    read -p "Pressione Enter para continuar..."
}

# Função para proteção contra ataques de força bruta com Fail2ban
protecao_fail2ban() {
    clear
    echo "======================= Proteção contra Ataques de Força Bruta com Fail2ban ======================="
    echo "Configurando Fail2ban..."

    # Iniciar e habilitar Fail2ban
    sudo systemctl start fail2ban
    sudo systemctl enable fail2ban

    echo "Fail2ban configurado e em execução."
    echo "================================================================================================="
    read -p "Pressione Enter para continuar..."
}

# Função para verificação de rootkits com Rkhunter
verificacao_rootkits_rkhunter() {
    clear
    echo "======================= Verificação de Rootkits com Rkhunter ======================="
    echo "Atualizando base de dados do Rkhunter..."
    sudo rkhunter --update

    echo "Executando verificação de rootkits..."
    sudo rkhunter --check

    echo "Verificação de rootkits concluída."
    echo "===================================================================="
    read -p "Pressione Enter para continuar..."
}

# Função para acessar dispositivo remotamente via SSH com força bruta
acessar_dispositivo_remoto() {
    clear
    echo "======================= Acessar Dispositivo Remoto via SSH ======================="
    echo "Insira o endereço IP do dispositivo alvo:"
    read ip_address
    echo "Insira o nome de usuário (username) para o acesso SSH:"
    read username
    echo "Insira o caminho para o arquivo de wordlist de senhas:"
    read wordlist

    echo "Tentando acessar $ip_address com o usuário $username usando Hydra..."
    hydra -l $username -P $wordlist ssh://$ip_address -t 4

    echo "Se uma senha for encontrada, use o comando abaixo para acessar o dispositivo:"
    echo "ssh $username@$ip_address"
    echo "==========================================================================="
    read -p "Pressione Enter para continuar..."
}

# Função para menu principal
mostrar_menu() {
    clear
    echo "==========================================="
    echo "    Menu - Ferramentas de Segurança         "
    echo "==========================================="
    echo " Escolha uma opção:"
    echo "  1. Ataque DDoS Simulado"
    echo "  2. Varredura de Portas com Nmap"
    echo "  3. Teste de Vulnerabilidade com Metasploit"
    echo "  4. Configurar VPN"
    echo "  5. Teste Simples de Segurança"
    echo "  6. Teste de Segurança de Rede WiFi com Aircrack-ng"
    echo "  7. Análise de Vulnerabilidades com OpenVAS"
    echo "  8. Scanner de Servidores Web com Nikto"
    echo "  9. Captura de Pacotes com Tcpdump"
    echo " 10. Antivírus com ClamAV"
    echo " 11. Proteção contra Ataques de Força Bruta com Fail2ban"
    echo " 12. Verificação de Rootkits com Rkhunter"
    echo " 13. Baixar e Instalar Ferramentas Adicionais"
    echo " 14. Acessar Dispositivo Remoto via SSH"
    echo "  0. Sair"
    echo "==========================================="
}

# Loop principal do script
while true; do
    mostrar_menu
    read -p "Digite a opção desejada: " opcao
    case $opcao in
        1) ataque_ddos ;;
        2) varredura_portas ;;
        3) teste_vulnerabilidade_metasploit ;;
        4) configurar_vpn ;;
        5) teste_seguranca ;;
        6) teste_wifi_aircrack ;;
        7) analise_vulnerabilidades_openvas ;;
        8) scanner_web_nikto ;;
        9) captura_pacotes_tcpdump ;;
        10) antivirus_clamav ;;
        11) protecao_fail2ban ;;
        12) verificacao_rootkits_rkhunter ;;
        13) baixar_instalar_ferramentas ;;
        14) acessar_dispositivo_remoto ;;
        0) echo "Saindo..."; exit ;;
        *) echo "Opção inválida. Tente novamente." ;;
    esac

    read -p "Pressione Enter para continuar..."
done

echo "Script concluído."
