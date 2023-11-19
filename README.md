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

Third session

Curso Solidity Básico Clase 3 | 28 de Octubre 2023


https://chain.link/ Sitio web de Chainlink
https://research.chain.link/whitepaper-v2.pdf Whitepaper de Chainlink

https://moralis.io/ 

Únete a mi grupo de WhatsApp. https://chat.whatsapp.com/HxIGTazmPGgKlxrsFVmZ6B

Henry Velásquez Y.
Juan Pablo Velasquez Camargo
Felipe Madrigal
Juan Pablo Bernal Cardona
Laura Stefania Zea
Karlimar Piza Gamboa
Rafael Antonio Camargo
Daniel Alberto Franco Cabrera
José Orlando Guevada Pedraza
Carlos Andres Monroy Martinez
Diego Felipe Alarcon Osorio
Rocío Elena Grajales Mesa
Hector David Puentes
Dannuver Cabezas
Alexander Aponte M.
Andres Giovanny Aponte
Juan Manuel Perez Rincon
Daniel Felipe Donoso Castiblanco
Sheryll Davina Vargas Ortiz
Martha Tatiana Díaz Urreg
Samuel Alejandro Bermúdez Piñeros
Ivan Santiago Romero Cepeda
Andres Felipe Ramos Reyes
Angélica Guevara Bernal
Juan Leonardo Ramirez Velasquez
Tannia Marcela Vega Buitrago
Juan Sebastian Vargas Ospina
Andrés Felipe Medina Bernal
Luis Miguel Taque Diaz
Juan Felipe Pedroza
Daniel Felipe Arevalo Benavides
Gustavo Alejandro Moreno Munevar
Sebastián Carrera
Juan Felipe Jimenez Pacheco
Carlos Esteban Leon Pinilla
JOse Antonio VIancha
David Mejia
David Millan
Julian Humberto Astroz Restrepo
David Mejia
jahir López cristancho
Miguel Angel Burgos Rojas
DAvid Fernando Mejia

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

Curso Solidity Básico Clase 4 | 4 de Noviembre 2023

https://bittensor.com/ proyecto interesante de IA y bockchain
https://youtu.be/HrQr8KQ102E Video Workshop 2 SpeedrunEthereum Reto 1 Noviembre 4
https://push.org/ Website Push Protocol
https://push.org/docs/ Documentación Push

https://www.openzeppelin.com/contracts Revisar esta página (Vamos a empezar a usar sus contratos en la próxima clase)

https://wizard.openzeppelin.com/ --- base de c}reación de smart contracts

Henry Velásquez Y
Daniel Alberto Franco Cabrera
Juan Pablo Velasquez Camargo
Julio César Arango
Andres Felipe Ramos
Karlimar Piza Gamboa
José Orlando Guevada Pedraza
Edgardo Mendez
Diego Felipe Alarcon Osorio
Daniel Felipe Arevalo Benavides
Adriana Manzano Rojas
Juan Sebastián Carrera Lozano
Jose Antonio Viancha
David Mejia Bonilla
Santiago Montenegro Novoa
Wilson Mauro Rojas Reales
Hector David Puentes Caceres
Julian Humberto Astroz Restrepo
Daniel Felipe Donoso Castiblanco
Sheryll Davina Vargas Ortiz
Dannuver Cabezas
Luis Miguel Taque Diaz
Sebastián Guaqueta
ALexander Aponte M.
joann cruz
David Millan
vanessa donoso
Carlos Andres Monroy Martinez
cz
Martha Tatiana Díaz Urrego
Juan Felipe Jimenez Pacheco
Samuel Alejandro Bermúdez Piñeros 
Ivan Santiago Romero Cepeda
Angelica Guevara
Miguel Angel Burgos Rojas
Oscar Eduardo Jaramillo
AndresAponta

==================

// SPDX-License-Identifier: MIT
pragma solidity 0.8.18;

contract Register {
        string private info;
        uint public countChanges; // = 0; (redundante)
        int256 withNegativeNumbers;
        address owner;

        constructor() {
            info = "Control Asistencia Tadeo";
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
        // - Usando JS pueden hacer queries a los eventos pasados
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



Al llamar a cada función, podemos ver que la función public usa mas gas, mientras que la función external usa menos.

La diferencia se debe a que en las funciones públicas, Solidity copia inmediatamente los argumentos de la matriz en la memoria, mientras que las funciones externas pueden leer directamente desde los datos de llamada. La asignación de memoria es costosa, mientras que la lectura de datos de llamadas es barata.

La razón por la que las funciones públicas necesitan escribir todos los argumentos en la memoria es que las funciones públicas pueden llamarse internamente, lo que en realidad es un proceso completamente diferente al de las llamadas externas. Las llamadas internas se ejecutan mediante saltos en el cód1os a la memoria. Por lo tanto, cuando el compilador genera el código para una función interna, esa función espera que sus argumentos estén ubicados en la memoria.

Para funciones externas, el compilador no necesita permitir llamadas internas, por lo que permite leer los argumentos directamente desde calldata, ahorrando el paso de copia.

En cuanto a las mejores prácticas, debe usar external si espera que la función solo se llame externamente y usar public si necesita llamar a la función internamente. Casi nunca tiene sentido utilizar el patrón this.f(), ya que requiere la ejecución de una CALL real, lo cual es costoso. Además, pasar matrices mediante este método sería mucho más costoso que pasarlas internamente.

Básicamente, verá beneficios de rendimiento con external cada vez que solo llame a una función externamente y pase una gran cantidad de datos de llamada (por ejemplo, matrices grandes).

Ejemplos para diferenciar:

public: todos pueden acceder

external: no se puede acceder internamente, solo externamente

internal: solo este contrato y los contratos que se derivan de él pueden acceder

private: solo se puede acceder desde este contrato

Version 1 Register Gas = 0.001661060005979816
Version 2 REgister Gas = 0.001661060005979816

///// El ejemplo | PushTokenDemo.sol

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.9;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/utils/Strings.sol";

// PUSH Comm Contract Interface
interface IPUSHCommInterface {
    function sendNotification(address _channel, address _recipient, bytes calldata _identity) external;
}

contract PushTokenDemo is ERC20 {

    using Strings for address;
    using Strings for uint;

    // EPNS COMM ADDRESS ON ETHEREUM KOVAN, CHECK THIS: https://docs.epns.io/developers/developer-tooling/epns-smart-contracts/epns-contract-addresses
    address public EPNS_COMM_ADDRESS = 0xb3971BCef2D791bc4027BbfedFb47319A4AAaaAa; // 

    constructor() ERC20("Push Token Demo", "PUSHDEMO") {
        _mint(msg.sender, 1000 * 10 ** uint(decimals()));
    }

    function transfer(address to, uint amount) override public returns (bool success) {
        address owner = _msgSender();
        _transfer(owner, to, amount);

        //"0+3+Hooray! ", msg.sender, " sent ", token amount, " PUSH to you!"
        IPUSHCommInterface(EPNS_COMM_ADDRESS).sendNotification(
            0x28400aa592483d6DC6776B82B25dD2E517863050, // from channel
            to, // to recipient, put address(this) in case you want Broadcast or Subset. For Targetted put the address to which you want to send
            bytes(
                string(
                    // We are passing identity here: https://docs.push.org/developers/developer-guides/sending-notifications/notification-payload-types/notification-standard-advanced/notification-identity
                    abi.encodePacked(
                        "0", // this is notification identity: https://docs.push.org/developers/developer-guides/sending-notifications/notification-payload-types/notification-standard-advanced/notification-identity
                        "+", // segregator
                        "3", // this is payload type: https://docs.push.org/developers/developer-guides/sending-notifications/notification-payload-types/notification-standard-advanced/notification-payload (1, 3 or 4) = (Broadcast, targetted or subset)
                        "+", // segregator
                        "Tranfer Alert", // this is notificaiton title
                        "+", // segregator
                        "Hooray! ", // notification body
                        owner.toHexString(), // notification body
                        " sent ", // notification body
                        (amount / (10 ** uint(decimals()))).toString(), // notification body
                        " PUSH to you!" // notification body
                    )
                )
            )
        );
        
        return true;
    }

}


https://app.push.org/send ----- PRODUCCION

https://medium.com/@sebasguaqueta.eth/una-gu%C3%ADa-f%C3%A1cil-para-entender-c%C3%B3mo-funcionan-las-comunicaciones-descentralizadas-a-trav%C3%A9s-de-push-y-de4cdbc7caf8


https://staging.push.org/channels ----- TESTNET


https://docs.push.org/developers/developer-guides/sending-notifications/using-subgraph-gasless




Curso Solidity Básico Clase 4 | 4 de Noviembre 2023

https://bittensor.com/ proyecto interesante de IA y bockchain
https://youtu.be/HrQr8KQ102E Video Workshop 2 SpeedrunEthereum Reto 1 Noviembre 4
https://push.org/ Website Push Protocol
https://push.org/docs/ Documentación Push

https://www.openzeppelin.com/contracts Revisar esta página (Vamos a empezar a usar sus contratos en la próxima clase)

https://wizard.openzeppelin.com/ --- base de creación de smart contracts

Henry Velásquez Y
Daniel Alberto Franco Cabrera
Juan Pablo Velasquez Camargo
Julio César Arango
Andres Felipe Ramos
Karlimar Piza Gamboa
José Orlando Guevada Pedraza
Edgardo Mendez
Diego Felipe Alarcon Osorio
Adriana Manzano Rojas
Juan Sebastián Carrera Lozano
Jose Antonio Viancha
David Mejia Bonilla
Santiago Montenegro Novoa
Wilson Mauro Rojas Reales
Hector David Puentes Caceres
Julian Humberto Astroz Restrepo
Daniel Felipe Donoso Castiblanco
Sheryll Davina Vargas Ortiz
Dannuver Cabezas
Luis Miguel Taque Diaz
Sebastián Guaqueta
ALexander Aponte M.
joann cruz
David Millan
Carlos Andres Monroy Martinez
cz
Martha Tatiana Díaz Urrego
Juan Felipe Jimenez Pacheco
Samuel Alejandro Bermúdez Piñeros 
Ivan Santiago Romero Cepeda
Angelica Guevara
Miguel Angel Burgos Rojas

==================

// SPDX-License-Identifier: MIT
pragma solidity 0.8.18;

contract Register {
        string private info;
        uint public countChanges; // = 0; (redundante)
        int256 withNegativeNumbers;
        address owner;

        constructor() {
            info = "Control Asistencia Tadeo";
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
        // - Usando JS pueden hacer queries a los eventos pasados
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



Al llamar a cada función, podemos ver que la función public usa mas gas, mientras que la función external usa menos.

La diferencia se debe a que en las funciones públicas, Solidity copia inmediatamente los argumentos de la matriz en la memoria, mientras que las funciones externas pueden leer directamente desde los datos de llamada. La asignación de memoria es costosa, mientras que la lectura de datos de llamadas es barata.

La razón por la que las funciones públicas necesitan escribir todos los argumentos en la memoria es que las funciones públicas pueden llamarse internamente, lo que en realidad es un proceso completamente diferente al de las llamadas externas. Las llamadas internas se ejecutan mediante saltos en el cód1os a la memoria. Por lo tanto, cuando el compilador genera el código para una función interna, esa función espera que sus argumentos estén ubicados en la memoria.

Para funciones externas, el compilador no necesita permitir llamadas internas, por lo que permite leer los argumentos directamente desde calldata, ahorrando el paso de copia.

En cuanto a las mejores prácticas, debe usar external si espera que la función solo se llame externamente y usar public si necesita llamar a la función internamente. Casi nunca tiene sentido utilizar el patrón this.f(), ya que requiere la ejecución de una CALL real, lo cual es costoso. Además, pasar matrices mediante este método sería mucho más costoso que pasarlas internamente.

Básicamente, verá beneficios de rendimiento con external cada vez que solo llame a una función externamente y pase una gran cantidad de datos de llamada (por ejemplo, matrices grandes).

Ejemplos para diferenciar:

public: todos pueden acceder

external: no se puede acceder internamente, solo externamente

internal: solo este contrato y los contratos que se derivan de él pueden acceder

private: solo se puede acceder desde este contrato

Version 1 Register Gas = 0.001661060005979816
Version 2 REgister Gas = 0.001661060005979816

///// El ejemplo | PushTokenDemo.sol

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.9;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/utils/Strings.sol";

// PUSH Comm Contract Interface
interface IPUSHCommInterface {
    function sendNotification(address _channel, address _recipient, bytes calldata _identity) external;
}

contract PushTokenDemo is ERC20 {

    using Strings for address;
    using Strings for uint;

    // EPNS COMM ADDRESS ON ETHEREUM KOVAN, CHECK THIS: https://docs.epns.io/developers/developer-tooling/epns-smart-contracts/epns-contract-addresses
    address public EPNS_COMM_ADDRESS = 0xb3971BCef2D791bc4027BbfedFb47319A4AAaaAa; // 

    constructor() ERC20("Push Token Demo", "PUSHDEMO") {
        _mint(msg.sender, 1000 * 10 ** uint(decimals()));
    }

    function transfer(address to, uint amount) override public returns (bool success) {
        address owner = _msgSender();
        _transfer(owner, to, amount);

        //"0+3+Hooray! ", msg.sender, " sent ", token amount, " PUSH to you!"
        IPUSHCommInterface(EPNS_COMM_ADDRESS).sendNotification(
            0x28400aa592483d6DC6776B82B25dD2E517863050, // from channel
            to, // to recipient, put address(this) in case you want Broadcast or Subset. For Targetted put the address to which you want to send
            bytes(
                string(
                    // We are passing identity here: https://docs.push.org/developers/developer-guides/sending-notifications/notification-payload-types/notification-standard-advanced/notification-identity
                    abi.encodePacked(
                        "0", // this is notification identity: https://docs.push.org/developers/developer-guides/sending-notifications/notification-payload-types/notification-standard-advanced/notification-identity
                        "+", // segregator
                        "3", // this is payload type: https://docs.push.org/developers/developer-guides/sending-notifications/notification-payload-types/notification-standard-advanced/notification-payload (1, 3 or 4) = (Broadcast, targetted or subset)
                        "+", // segregator
                        "Tranfer Alert", // this is notificaiton title
                        "+", // segregator
                        "Hooray! ", // notification body
                        owner.toHexString(), // notification body
                        " sent ", // notification body
                        (amount / (10 ** uint(decimals()))).toString(), // notification body
                        " PUSH to you!" // notification body
                    )
                )
            )
        );
        
        return true;
    }

}


https://app.push.org/send ----- PRODUCCION

https://medium.com/@sebasguaqueta.eth/una-gu%C3%ADa-f%C3%A1cil-para-entender-c%C3%B3mo-funcionan-las-comunicaciones-descentralizadas-a-trav%C3%A9s-de-push-y-de4cdbc7caf8


https://staging.push.org/channels ----- TESTNET


https://docs.push.org/developers/developer-guides/sending-notifications/using-subgraph-gasless


Fifth Session

Bogota 11 de Noviembre 2023 Clase 5 Solidity Básico

Cuando se va a realizar la Hackaton, si se va a hacer? Si, regístrate aquí
https://tally.so/r/wo2eaO

=============
Asistencia
ºHenry Velásquez Y. (Bonfire)
Oscar Riojas TK-0xe582B2aaf93cE495dB591dAE1f5E8ECEF33e214f WLL: 0x1d870f1210e66cba98093682b84d4491Ec04141b
ºKarlimar Piza Gamboa
ºDiego Felipe Alarcon Osorio
ºDaniel Felipe Arevalo Benavides
ºDaniel Alberto Franco Cabrera
ºCarlos Andres Monroy Martinez
ºWilson Mauro Rojas Reales
ºGustavo Alejandro Moreno Munevar
ºJose Antonio Viancha Monroy
ºMiguel Ángel Burgos Rojas. TK 0x4Bb9d5938D6977C1Ef6E6875e3bc7FA596F8801A 
WLL 0xD95310a95D1698cE0423E4145D58925ED0E74C8b
ºLuis Miguel Taque Diaz
ºAdriana Manzano Rojas
ºJoann Cruz
ºSantiago Montenegro tk - 0xfCC8316363b1b37B85B9d63a6C940a253B885338 
Wll: 0xD6baeEe78bd7F36B550D9A64fBc4383dF3AF73de 

ºDannuver Cabezas 0x0443925bd3B13CB62bd2beec06d325d7b889E9CE
ºAndres Felipe Ramos Reyes 0x526A29B0daBf45a4a5c7587A15BCa97547392fC2
ºJuan Sebastian Carrera Lozano
ºAlexander Aponte M.
ºWilliam Stiven Forero Pardo.   0x91B00Cdecb7341208763453e3586f0173aCe3ffB

ºAndrés Felipe Medina Bernal   0x4Ac05fb296E92c972C328dB176232a10b20b8b04
ºRocío Elena Grajales Mesa
ºIvan Santiago Romero Cepeda tk- 0x6Cfa0c12761e4D62224eC6b46a502cC6215F50B9
CONTRATO- 0x5D6Ae4a2D8114D4627DeFeea44AbB9aDC0f49B8E
ºJuan Pablo Velasquez Camargo
ºDaniel Felipe Donoso Castiblanco
ºJulian Humberto Astroz Restrepo
ºJuan Leonardo Ramirez Velasquez TK: 0xF1F68fe21a73C507B2747e5C100a0CC202810eE7 W: 0xBDB91F50919b1Ae3F74d8fAa844C69C4f45CbC8D
ºJuan Sebastian Vargas Ospina
ºDavid Felipe Millan Mayorga
ºJuan Sebastian Hurtado Palacios JTK: 0x6d158A8670a5E7379dF9FC4EdA05980A393Bced3
Wallet: 0x15ef07023FB0C5Eaf3C6BB76915d079e0F5e8470
ºLuis hernando Martinez Poveda
ºAndrea Jiménez Guevara (XX asistencias)
ºHector David Puentes Caceres
ºSheryll Davina Vargas Ortiz
ºMiyyer Andres Gaitan
ºAndres Felipe Carbonell Amaya
ºTannia Marcela Vega Buitrago
ºMartha Tatiana Díaz Urrego
Wallet:0x60f96B78731834dAa395b104AbC126C7cE7fcd32
Contrato: 0x9a5c00e1cc458450fd5b705a702add399548ad77
ºOscar Eduardo Jaramillo
ºJuan Felipe Jimenez Pacheco PJK- 
0x1E94aA61A9a4828931d62a4307Cf10bC18364d8F
ºJuan Manuel Perez Rincon
ºAngélica Guevara Bernal   
      0x7D41497939490d369338e638C98B9935D7a3FcA8
ºPaula Alejandra Buitrago Pineda 
ºAndres Aponte
ºCristian Chaparro
Julio Cesar Arango 0x47Cc9Ae1B382B4B6db3cAA500F78208490eB9Ce9 Wallet
0x7887822803A6cCae4c83CdfAB0C9211c97831fd4 Token JulioCesar JCR
Wilfredo_Caro 0xAc7e174DF113092dbE2459C0989a7947b449Aa68

 0xdc7FbbDBd66Ab1896593AdB2F35e8c20C0983c06
 0x1d870f1210e66cba98093682b84d4491Ec04141b
 0x5526Be6A5d6648FE015c41Ac9a3C077dC6FC632f

=============

 
Block

Transaction Properties

- block.coinbase (address payable) Current block miner’s address
- block.difficulty (uint) Current block difficulty
- block.number (uint): Current block number
- blockhash(uint blockNumber) returns (bytes32) Gives hash of the given block and will only work for the 256
most recent block due to the reason of scalability.
- block.timestamp (uint): Current block timestamp as seconds since unix epoch
- gasleft() returns (uint256): Remaining gas
- msg.sender (address payable) Sender of the message (current call)
- msg.sig (bytes4) First four bytes of the calldata (i.e. function identifier)

- tx.gasprice (uint) Gas price of the transaction
- block.gaslimit (uint) Current block gaslimit
- tx.origin (address payable) Sender of the transaction (full call chain)
- msg.data (bytes calldata) Complete calldata

Block difficulty: https://2miners.com/es/ethw-network-difficulty , https://www.okx.com/es-es/learn/ethereum-difficulty-bomb
https://dappnode.com/ Hardware para Staking de Ethereum

==========

//SPDX-License-Identifier:MIT
pragma solidity ^0.8.16;

contract test_1 {

        address public block_now;
        uint public block_diff;
        uint public block_num;
        bytes32 public block_hash; // tipo de dato para el blockhash
        uint public time_stamp;
        uint public gas_left;
        address public sender;
        bytes4 public sig_id;
        uint public gas_limit;

        function update() public {

            block_now = block.coinbase;
            block_diff = block.difficulty;
            block_num = block.number;
            block_hash = blockhash(block_num);
            time_stamp = block.timestamp;
            gas_left = gasleft();
            sender = msg.sender;
            sig_id = msg.sig;
            gas_limit = block.gaslimit;

        }
}

===================================================================================

https://ethereum.org/es/developers/docs/standards/tokens/erc-20/

Tokenomics es la combinación ente «token» y «economía». Se trata de una forma de englobar aquellos elementos que hacen que una criptomoneda sea valiosa e interesante para los inversores. Incluye todos los aspectos, desde cómo se emite, hasta su utilidad, oferta, distribución y demanda, entre otras cosas.

https://docs.openzeppelin.com/contracts/5.x/wizard

ERC-20
Obligatorias
Name y symbol

No obligatorias
Premint: tokens que se crean al iniciar
Mintable: poder crear mas tokens
Burnable: quemar tokens
Pausable: EIP-2612
Votes: governanza

Tener en cuenta los decimales

https://quickswap.exchange/#/
https://testnet.unilend.finance/
https://medium.com/coinosis/c%C3%B3mo-usar-decimales-en-solidity-915f890914ee

Smart contract BAT 
https://etherscan.io/address/0x0d8775f648430679a709e98d2b0cb6250d2887ef#code

Sobre el crash de FTX aquí les dejo:
    https://www.criptonoticias.com/negocios/10-claves-entender-que-paso-ftx/
  Bonfire
  
  https://www.youtube.com/watch?v=o92MEv_FlXA&t=12s&ab_channel=UNPOCOMEJOR
  el dinero explicado en Netflix
  


Sixth Session


Curso Solidity Básico Clase 6 | 18 de Noviembre de 2023

Registro Hackathon Web3 https://tally.so/r/wo2eaO

Contrato ERC-721 https://docs.openzeppelin.com/contracts/5.x/erc721

DISCORD ETH BOGOTA https://discord.gg/vXzhJytpTs

https://aragon.org/ -> DAO
https://www.tally.xyz/ -> DAO

¿Qué son los NFTs? Por DW en Español https://www.youtube.com/watch?v=js32a08YUds
https://openzeppelin.com/contracts -> Plataforma para verificación de contrato y estandares'p
https://docs.openzeppelin.com/contracts/5.x/wizard -> Plantilla para contratos ya auditada
https://web3.storage/login/ ->Para guardar de forma descentralizada 100
https://www.pinata.cloud/ -> 
https://www.lighthouse.storage ->Almacenamiento a 100 años/encriptación nativa
https://files.lighthouse.storage/ -> Almacenamiento desentralizado chevere
www.jsoneditoronline.org -> Editar archivos json online
https://docs.opensea.io/docs/metadata-standards ->Seguir un standard para vender NFTs en Opensea
https://testnets.opensea.io/ ->

CID = Content Identifier (https://docs.ipfs.tech/concepts/content-addressing/)

=======
https://gateway.lighthouse.storage/ipfs/QmcSRs1iXZt79ggtpdgPxTeEiR9Za7xYPE1K75f2M1QuvV

https://testnets.opensea.io/assets/mumbai/0x10573534c7a97253c7c3074d894700608841754e/1/

Estructura JSON

{
  "description": "La mejor imagen de ",
  "external_url": "https://gateway.lighthouse.storage/ipfs/QmZi8MJAfYSB4iK6DkeahEDZgaSmieCY2pEvFD6cy6GKr5",
  "image": "https://gateway.lighthouse.storage/ipfs/QmZi8MJAfYSB4iK6DkeahEDZgaSmieCY2pEvFD6cy6GKr5",
  "name": "Picasso"
}

=======

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract Picasso2 is ERC721, ERC721URIStorage, Ownable {
    uint256 private _nextTokenId;

    constructor(address initialOwner)
        ERC721("Picasso2", "PCSS")
        Ownable(initialOwner)
    {}

    function safeMint(address to, string memory uri) public onlyOwner {
        uint256 tokenId = _nextTokenId++;
        _safeMint(to, tokenId);
        _setTokenURI(tokenId, uri);
    }

    // The following functions are overrides required by Solidity.

    function tokenURI(uint256 tokenId)
        public
        view
        override(ERC721, ERC721URIStorage)
        returns (string memory)
    {
        return super.tokenURI(tokenId);
    }

    function supportsInterface(bytes4 interfaceId)
        public
        view
        override(ERC721, ERC721URIStorage)
        returns (bool)
    {
        return super.supportsInterface(interfaceId);
    }
}

=======

Nuestro activo digital, que puede ser video, audio, contenido propio ...
Subir el activo digital a IPFS (lighthouse, Web3.storage, pinata.cloud)
Crear el json con la metadata con el estandar de OpenSea y remplazar las uris
Subir el json a IPFS (https://gateway.lighthouse.storage/ipfs/"copiar el CID")
Crear el contrato ERC-721 en open zeppelin
Ejecutar la función SAFE MINT y decrile la uri de nuestra metadata json 
Si quiero adjuntarlo en mi billetera  (Verificar los contratos antes)




<<<<<<<<<<<>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>Speedrun Sessions<<<<<<<<<<<<<<<<<<<<<>>>>>>>>>>>>>>>>>>>>>


<--Speedrun first session-->
https://youtu.be/johpGi8HS4U?si=W4LpvFTZ0X1aTbTT
https://youtu.be/johpGi8HS4U?si=sX3LZToM4ktS2Ud9

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

