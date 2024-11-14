<h1>Configuración Cliente e Servidor DNS</h1>
 <p><strong>Autor:</strong> Iván Magariños Brea</p>
 <h2>Proposta</h2>
 <p>Engadir un servizo que se conectará ao DNS no ficheiro <code>docker-compose.yml</code>
 do proxecto anterior.</p>
 <h2>Recursos</h2>
 <p>Utilizaremos o repositorio de Damián para implementar o seguinte ficheiro de
 configuración.</p>
 <h3>Modificacións en <code>docker-compose.yml</code></h3>
 <p>A continuación detállanse os servizos e configuracións necesarios para executar o contedor con
 Bind9 (servidor DNS) e un cliente Alpine.</p>
 <h4>Definición de Servizos</h4>
 <p><code>yaml
 services:
  # Servizos que se executarán no contedor
  bind9: # Nome do servizo DNS
    image: internetsystemsconsortium/bind9:9.18 # Imaxe utilizada no contedor
    container_name: Practica5_bind9 # Nome específico do contedor
    ports:
      # Mapeo de portos host:contedor
      - 54:53/udp
      - 54:53/tcp
      - 127.0.0.1:953:953/tcp # Só permite conexións dende localhost
    volumes:
      # Directorios onde se montará o contedor
      - ./etc/bind:/etc/bind
      - ./var/cache/bind:/var/cache/bind
      - ./var/lib/bind:/var/lib/bind
    restart: always # Reinicia o contedor en certas condicións
 </code></p>
 <h4>Configuración de Redes</h4>
 <p>Primeiro, engadimos unha rede na configuración para especificarlla a cada contedor. Existen
 dúas maneiras de facelo:</p>
 <ol>
 <li><p><strong>Despregar a rede xunto coa creación de contedores:</strong></p>
 <p><code>yaml
 networks:
  P6_network: # Nome da rede
    driver: bridge # Tipo de rede
    ipam:
      config:
        - subnet: 172.18.0.0/16 # Rango de direccións IP
          ip_range: 172.18.0.0/24 # Subrango de direccións
          gateway: 172.18.8.254 # Dirección do gateway
 </code></p></li>
 </ol>
