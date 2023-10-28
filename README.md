# SolidityCourseNotes
Notes from Solidity Basics course

<--First session-->

https://youtu.be/OrPTgeMsZXM?si=99Xl5w4w5NeNkBvO)


Curso Solidity Básico Clase 1 | 14 de Octubre 2023

https://andersbrownworth.com/blockchain/block

https://remix.ethereum.org/

https://solidity-by-example.org/

https://metamask.io


https://speedrunethereum.com/

- REMIX: https://remix.ethereum.org/

- SOLIDITY BY EXAMPLES: https://solidity-by-example.org/

- METAMASK: https://metamask.io/

- DEMO BLOCKCHAIN: https://andersbrownworth.com/blockchain/block

<--Second session-->

https://youtu.be/DDOgJmSaW8Y?si=Le1oBRNX9RFKYbXd

- GAS DE ETHEREUM: https://ethereum.org/es/developers/docs/gas/

- LA VISIÓN DE ETHEREUM: https://ethereum.org/es/roadmap/vision/

- FAUSET DE SEPOLIA: https://sepoliafaucet.com/

- ETHERSCAN DE SEPOLIA: https://sepolia.etherscan.io/

- ÁRBOL DE MERKLE: https://blog.ethereum.org/2015/11/15/merkling-in-ethereum

Curso Solidity Básico Clase 2 | 21 de Octubre 2023

https://metamask.io   Crea tu billetera EOA non-custodial
https://remix.ethereum.org/   Herramienta para escribir código en Solidity
https://solidity-by-example.org/   Ejemplos y tutoriales para aprender Solidity
https://sepoliafaucet.com/ Faucet para optener tokens de prueba
https://speedrunethereum.com/.  Retos de Speedrun Ethereum, iniciaremos el jueves 26 de Octubre a las 6:00 PM en la Universidad Jorge Tadeo Lozano
https://sepolia.etherscan.io/ escaner de la blockchain
https://blog.ethereum.org/2015/11/15/merkling-in-ethereum Merkle Trees

https://app.deform.cc/form/37bb4b2a-0f5c-4365-b17c-2ff1d163e0c3/ Hackathon Developer DAO x Chainbase, una expedición técnica de una semana que se llevará a cabo desde el 23 hasta el 30 de octubre de 2023.



Descentralización -> Escalabilidad -> Seguridad
https://ethereum.org/es/roadmap/vision/


===================

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract HolaMundo {
    string public holaMundo = "Hola Mundo desde la Tadeo!";
}

===================


===================

// SPDX-License-Identifier: MIT
pragma solidity 0.8.20;

contract MiPrimerContrato {

    string saludo = "Saludos desde Bogota";


    function leerSaludo() public view returns (string memory) {
        return saludo;
    }


    function guardarSaludo(string memory nuevoSaludo) public {
        saludo = nuevoSaludo;
    }
}

==================


Tarea: hacer el contrato MiPrimerContrato pero incluyendo el correo


==================

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Primitives {
    bool public boo = true;

    /*
        uint8   ranges from 0 to 2 ** 8 - 1
        uint16  ranges from 0 to 2 ** 16 - 1
        uint256 ranges from 0 to 2 ** 256 - 1
    */
    uint8 public u8 = 1;
    uint public u256 = 456;
    uint public u = 123; // uint is an alias for uint256

    /*    
        int256 ranges from -2 ** 255 to 2 ** 255 - 1
        int128 ranges from -2 ** 127 to 2 ** 127 - 1
    */

    int8 public menos_uno= -1;
    int public ccs = 456;
    int public menos123 = -123; // int is same as int256

    int public minInt = type(int).min;
    int public maxInt = type(int).max;

    address public addr = 0xCA35b7d915458EF540aDe6068dFe2F44E8fa733c;

    /*
        datos tipo bytes
    */
    bytes1 a = 0xb5; //  [10110101]
    bytes1 b = 0x56; //  [01010110]

    // Default values
    // Unassigned variables have a default value
    bool public defaultBoo; // false
    uint public defaultUint; // 0
    int public defaultInt; // 0
    address public defaultAddr; // 0x0000000000000000000000000000000000000000
}

==================


<--Third session-->

Curso Solidity Básico Clase 3 | 28 de Octubre 2023





==================
//SPDX-License-Identifier: MIT
pragma solidity 0.8.19;


contract Contract_tarea {

        string nombre = "Pablo Perez";
        string correo = "mail@mail.com";

        function leerNombre() public view returns (string memory) {
            return nombre;
        }

        function guardarNombre(string memory nuevoNombre) public {
            nombre = nuevoNombre;
        }

        function leerCorreo() public view returns (string memory) {
            return correo;
        }

        function guardarCorreo(string memory nuevoCorreo) public {
            correo = nuevoCorreo;
        }

}

==================

==================

// SPDX-License-Identifier: MIT
pragma solidity 0.8.18;

contract Register {
        string private info;
        uint public countChanges; // = 0; (redundante)
        int256 withNegativeNumbers;
        address owner;

        constructor() {
            info = "Oscar";
            owner = msg.sender;
        }

        function getInfo() public view returns(string memory) {
            return info;
        }

        // MODIFIER
        // Sirve para extender la funcionalidad de un método
        // Se pueden hacer validaciones (multiples) antes de que se ejecute el metodo
        modifier ValidarQueNoEsElOwner {
            // Require
            require(owner == msg.sender, "No eres el owner del contrato");
            // Revert
            // require(owner == msg.sender, "No eres el owner del contrato");
            // if (owner != msg.sender) revert("No eres el owner del contrato");
            // // Revert con error personalizado
            // // Nuevo estandar - menos costos
            // if (owner != msg.sender) revert NoEsElOwner();
        
            // Indica se debe ejecutar el metodo
            _;
        }
    
        // error NoEsElOwner();
        // Este error se dispara en revert NoEsElOwner();

        modifier ValidarSiCadenaEstaVacia(string memory _info) {
            require(bytes(_info).length > 0, "La cadena esta vacia");
            _;
        }

        // Eventos
        // - Es una manera de propagar informacion del smart contract hacia afuera
        // - Otros agentes (backned, frontendt) pueden suscribirse para escuchar los eventos
        // - Todos los eventos se guardan en el blockchain
        // - Se usa como storage barato para guardar informacion
        // - Usando JS pueden hacer queries (como si fuera SQL) a los eventos pasados
        // - Otros contratos inteligentes no pueden escuchar eventos
        event InfoChange(string oldInfo, string newInfo);

        function setInfo(
            string memory _info
        ) external  
            ValidarQueNoEsElOwner 
            ValidarSiCadenaEstaVacia(_info) {
                emit InfoChange(info, _info);
                info = _info;
                ++countChanges; // <= este es el menos costos
        }

}

==================





<--Speedrun first session-->
https://youtu.be/johpGi8HS4U?si=W4LpvFTZ0X1aTbTT

HOY INICIA EL SPEED RUN ETHEREUM
---------------
Hora: 6:00 a 9:00 pm
Lugar: Universidad Jorge tadeo Lozano
- Edificio M7a, Aula Oval, los días Oct : 26, Nov: 2, 9, 23, 30
- Edificio M26, Aula Activa 3, el día Nov:16
-----------------
NO OLVIDAR
Para la primera sesión de Speed Run Ethereum es crucial tener lo siguiente:

👇👇👇

Checkpoint 0: 📦 Environment 📚
Before you begin, you need to install the following tools:

Node (v18 LTS)
Yarn (v1 or v2+)
Git
Visual Studio Code

Then download the challenge to your computer and install dependencies by running:

git clone https://github.com/scaffold-eth/se-2-challenges.git challenge-0-simple-nft

