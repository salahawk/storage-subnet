### *Guía Paso a Paso para Ejecutar en la Red de Pruebas (Testnet)*

*Objetivo*: Esta guía te ayudará a configurar y ejecutar tu mecanismo en la red de pruebas de Bittensor.

*Precaución*: Asegúrate de no exponer tus claves privadas y de usar solo tu billetera de testnet. No utilices las mismas contraseñas que tu billetera principal (mainnet).

---

#### *1. Preparación del Entorno*

- *Requisitos*:
  - Tener instalada la rama `revolution` de Bittensor.

- *Instalación*:
  bash
  cd ..
  git clone https://github.com/opentensor/bittensor-subnet-template.git
  cd bittensor-subnet-template
  python -m pip install -e .
  

#### *2. Configuración de Billeteras*

- *Crear billeteras* para el propietario de tu subred, tu validador y tu minero:
  bash
  btcli wallet new_coldkey --wallet.name owner
  btcli wallet new_coldkey --wallet.name miner
  btcli wallet new_hotkey --wallet.name miner --wallet.hotkey default
  btcli wallet new_coldkey --wallet.name validator
  btcli wallet new_hotkey --wallet.name validator --wallet.hotkey default
  

#### *3. Verificación de Costos*

- *Consultar el precio actual* para crear una subred:
  bash
  btcli subnet lock_cost --subtensor.network test
  

#### *4. Obtención de Tokens (Opcional)*

- Si necesitas tokens para la red de pruebas, puedes obtenerlos así:
  bash
  btcli wallet faucet --wallet.name owner --subtensor.network test
  

#### *5. Registro en la Red*

- *Compra un slot* para registrar tu subred:
  bash
  btcli subnet create --subtensor.network test
  

- *Registra tus claves* de minero:
  bash
  btcli subnet register --wallet.name miner --wallet.hotkey default --subtensor.network test
- *Registra tus claves* de validador:
  btcli subnet register --wallet.name validator --wallet.hotkey default --subtensor.network test
  

#### *6. Verificación de Registro*

- *Comprueba* que tus claves han sido registradas:
  bash
  btcli wallet overview --wallet.name validator --subtensor.network test
  btcli wallet overview --wallet.name miner --subtensor.network test
  

#### *7. Configuración de la Subred*

- *Edita* los argumentos por defecto en `template/_init_.py`:
  Asegúrate de que `NETUID=12` y `CHAIN_ENDPOINT=ws://127.0.0.1:9946`.

#### *8. Instalación de Requisitos Adicionales*

- *Navega al directorio y instala los requisitos*:
  bash
  cd storage-subnet
  pip install -r requirements.txt
  

- *Instala las dependencias necesarias*:
  bash
  apt-get install python-virtualenv python-dev librocksdb-dev
  virtualenv venv
  source venv/bin/activate
  pip install python-rocksdb
  

#### *9. Configuración para Ubuntu*

- *Actualiza GCC para soporte C++11*:
  bash
  sudo apt-get update
  sudo apt-get upgrade gcc
  

- *Instala las dependencias*:
  bash
  sudo apt-get install libgflags-dev libsnappy-dev zlib1g-dev libbz2-dev liblz4-dev libzstd-dev
  

- *Clona y compila RocksDB*:
  bash
  git clone https://github.com/facebook/rocksdb.git
  cd rocksdb
  make static_lib
  

#### *10. Ejecución de Minero y Validador*

- *Ejecuta el minero*:
  bash
  python storage-subnet/neurons/miner.py -- --wallet.name miner --wallet.hotkey default --axon.port $RUNPOD_TCP_PORT_70003 --netuid 12 --logging.trace --logging.debug --subtensor.network test
  

- *Ejecuta el validador*:
  bash
  python storage-subnet/neurons/validator.py -- --wallet.name validator --wallet.hotkey default --axon.port $RUNPOD_TCP_PORT_70003 --netuid 12 --logging.trace --logging.debug --subtensor.network test
  

#### *11. Detener Nodos*

- Si deseas *detener* tus nodos, simplemente presiona `CTRL + C` en la terminal donde se están ejecutando.

---

Con estos pasos, deberías tener todo listo para ejecutar tu mecanismo en la red de pruebas de Bittensor. ¡Buena suerte!