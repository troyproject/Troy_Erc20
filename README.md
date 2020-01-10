# Troy_Erc20

Contract Name: TROY 

Compiler Version: v0.5.8+commit.23d335f2

------


#### Contract Source Code (Solidity)

```go
/**
 *Submitted for verification at Etherscan.io on 2019-06-20
*/

pragma solidity >=0.4.22 <0.6.0;

interface IERC20 {
    function transfer(address to, uint256 value) external returns (bool);

    function approve(address spender, uint256 value) external returns (bool);

    function transferFrom(address from, address to, uint256 value) external returns (bool);

    function totalSupply() external view returns (uint256);

    function balanceOf(address who) external view returns (uint256);

    function allowance(address owner, address spender) external view returns (uint256);

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract Ownable {
    address private _owner;
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
    constructor () internal {
        _owner = msg.sender;
        emit OwnershipTransferred(address(0), _owner);
    }
    function owner() public view returns (address) {
        return _owner;
    }
    
    modifier onlyOwner() {
        require(isOwner());
        _;
    }
    
    function isOwner() public view returns (bool) {
        return msg.sender == _owner;
    }
    
    function renounceOwnership() public onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }
    
    function transferOwnership(address newOwner) public onlyOwner {
        _transferOwnership(newOwner);
    }
   
    function _transferOwnership(address newOwner) internal {
        require(newOwner != address(0));
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}


contract SafeMath {
  function safeMul(uint256 a, uint256 b) internal pure returns (uint256) {
    uint256 c = a * b;
    assert(a == 0 || c / a == b);
    return c;
  }

  function safeDiv(uint256 a, uint256 b) internal pure returns (uint256) {
    assert(b > 0);
    uint256 c = a / b;
    assert(a == b * c + a % b);
    return c;
  }

  function safeSub(uint256 a, uint256 b) internal pure returns (uint256) {
    assert(b <= a);
    return a - b;
  }

  function safeAdd(uint256 a, uint256 b) internal pure returns (uint256) {
    uint256 c = a + b;
    assert(c>=a && c>=b);
    return c;
  }

}

contract TROY is Ownable, SafeMath, IERC20{
    string public name;
    string public symbol;
    uint8 public decimals;
    uint256 public totalSupply;

    mapping (address => uint256) public balanceOf;
    mapping (address => mapping (address => uint256)) public allowance;
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    constructor()  public  {
        balanceOf[msg.sender] = 1000000000000000000000000000;
        totalSupply = 1000000000000000000000000000;
        name = "TROY";
        symbol = "TROY";
        decimals = 18;
		
    }

    function transfer(address _to, uint256 _value) public returns (bool) {
        require(_to != address(0));
		require(_value > 0); 
        require(balanceOf[msg.sender] >= _value);
        require(balanceOf[_to] + _value >= balanceOf[_to]);
		uint previousBalances = balanceOf[msg.sender] + balanceOf[_to];		
        balanceOf[msg.sender] = SafeMath.safeSub(balanceOf[msg.sender], _value);
        balanceOf[_to] = SafeMath.safeAdd(balanceOf[_to], _value);
        emit Transfer(msg.sender, _to, _value);
		assert(balanceOf[msg.sender]+balanceOf[_to]==previousBalances);
        return true;
    }

    function approve(address _spender, uint256 _value) public returns (bool success) {
		require((_value == 0) || (allowance[msg.sender][_spender] == 0));
        allowance[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;
    }

    function transferFrom(address _from, address _to, uint256 _value) public returns (bool success) {
        require (_to != address(0));
		require (_value > 0); 
        require (balanceOf[_from] >= _value) ;
        require (balanceOf[_to] + _value > balanceOf[_to]);
        require (_value <= allowance[_from][msg.sender]);
        balanceOf[_from] = SafeMath.safeSub(balanceOf[_from], _value);
        balanceOf[_to] = SafeMath.safeAdd(balanceOf[_to], _value);
        allowance[_from][msg.sender] = SafeMath.safeSub(allowance[_from][msg.sender], _value);
        emit Transfer(_from, _to, _value);
        return true;
    }
}
```



#### Contract ABI

```
[{"constant":true,"inputs":[],"name":"name","outputs":[{"name":"","type":"string"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"name":"_spender","type":"address"},{"name":"_value","type":"uint256"}],"name":"approve","outputs":[{"name":"success","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name":"totalSupply","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"name":"_from","type":"address"},{"name":"_to","type":"address"},{"name":"_value","type":"uint256"}],"name":"transferFrom","outputs":[{"name":"success","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name":"decimals","outputs":[{"name":"","type":"uint8"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[{"name":"","type":"address"}],"name":"balanceOf","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[],"name":"renounceOwnership","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name":"owner","outputs":[{"name":"","type":"address"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"isOwner","outputs":[{"name":"","type":"bool"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"symbol","outputs":[{"name":"","type":"string"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"name":"_to","type":"address"},{"name":"_value","type":"uint256"}],"name":"transfer","outputs":[{"name":"","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[{"name":"","type":"address"},{"name":"","type":"address"}],"name":"allowance","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"name":"newOwner","type":"address"}],"name":"transferOwnership","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"inputs":[],"payable":false,"stateMutability":"nonpayable","type":"constructor"},{"anonymous":false,"inputs":[{"indexed":true,"name":"from","type":"address"},{"indexed":true,"name":"to","type":"address"},{"indexed":false,"name":"value","type":"uint256"}],"name":"Transfer","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"name":"owner","type":"address"},{"indexed":true,"name":"spender","type":"address"},{"indexed":false,"name":"value","type":"uint256"}],"name":"Approval","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"name":"previousOwner","type":"address"},{"indexed":true,"name":"newOwner","type":"address"}],"name":"OwnershipTransferred","type":"event"}]
```



#### Contract Creation Code

```
60806040523480156200001157600080fd5b50336000806101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055506000809054906101000a900473ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16600073ffffffffffffffffffffffffffffffffffffffff167f8be0079c531659141344cd1fd0a4f28419497f9722a3daafe3b4186f6b6457e060405160405180910390a36b033b2e3c9fd0803ce8000000600560003373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff168152602001908152602001600020819055506b033b2e3c9fd0803ce80000006004819055506040518060400160405280600481526020017f54524f5900000000000000000000000000000000000000000000000000000000815250600190805190602001906200017e929190620001ef565b506040518060400160405280600481526020017f54524f590000000000000000000000000000000000000000000000000000000081525060029080519060200190620001cc929190620001ef565b506012600360006101000a81548160ff021916908360ff1602179055506200029e565b828054600181600116156101000203166002900490600052602060002090601f016020900481019282601f106200023257805160ff191683800117855562000263565b8280016001018555821562000263579182015b828111156200026257825182559160200191906001019062000245565b5b50905062000272919062000276565b5090565b6200029b91905b80821115620002975760008160009055506001016200027d565b5090565b90565b6112cd80620002ae6000396000f3fe608060405234801561001057600080fd5b50600436106100cf5760003560e01c8063715018a61161008c57806395d89b411161006657806395d89b4114610353578063a9059cbb146103d6578063dd62ed3e1461043c578063f2fde38b146104b4576100cf565b8063715018a6146102dd5780638da5cb5b146102e75780638f32d59b14610331576100cf565b806306fdde03146100d4578063095ea7b31461015757806318160ddd146101bd57806323b872dd146101db578063313ce5671461026157806370a0823114610285575b600080fd5b6100dc6104f8565b6040518080602001828103825283818151815260200191508051906020019080838360005b8381101561011c578082015181840152602081019050610101565b50505050905090810190601f1680156101495780820380516001836020036101000a031916815260200191505b509250505060405180910390f35b6101a36004803603604081101561016d57600080fd5b81019080803573ffffffffffffffffffffffffffffffffffffffff16906020019092919080359060200190929190505050610596565b604051808215151515815260200191505060405180910390f35b6101c561071b565b6040518082815260200191505060405180910390f35b610247600480360360608110156101f157600080fd5b81019080803573ffffffffffffffffffffffffffffffffffffffff169060200190929190803573ffffffffffffffffffffffffffffffffffffffff16906020019092919080359060200190929190505050610721565b604051808215151515815260200191505060405180910390f35b610269610b58565b604051808260ff1660ff16815260200191505060405180910390f35b6102c76004803603602081101561029b57600080fd5b81019080803573ffffffffffffffffffffffffffffffffffffffff169060200190929190505050610b6b565b6040518082815260200191505060405180910390f35b6102e5610b83565b005b6102ef610c53565b604051808273ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200191505060405180910390f35b610339610c7c565b604051808215151515815260200191505060405180910390f35b61035b610cd3565b6040518080602001828103825283818151815260200191508051906020019080838360005b8381101561039b578082015181840152602081019050610380565b50505050905090810190601f1680156103c85780820380516001836020036101000a031916815260200191505b509250505060405180910390f35b610422600480360360408110156103ec57600080fd5b81019080803573ffffffffffffffffffffffffffffffffffffffff16906020019092919080359060200190929190505050610d71565b604051808215151515815260200191505060405180910390f35b61049e6004803603604081101561045257600080fd5b81019080803573ffffffffffffffffffffffffffffffffffffffff169060200190929190803573ffffffffffffffffffffffffffffffffffffffff169060200190929190505050611128565b6040518082815260200191505060405180910390f35b6104f6600480360360208110156104ca57600080fd5b81019080803573ffffffffffffffffffffffffffffffffffffffff16906020019092919050505061114d565b005b60018054600181600116156101000203166002900480601f01602080910402602001604051908101604052809291908181526020018280546001816001161561010002031660029004801561058e5780601f106105635761010080835404028352916020019161058e565b820191906000526020600020905b81548152906001019060200180831161057157829003601f168201915b505050505081565b60008082148061062257506000600660003373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002060008573ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002054145b61062b57600080fd5b81600660003373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002060008573ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff168152602001908152602001600020819055508273ffffffffffffffffffffffffffffffffffffffff163373ffffffffffffffffffffffffffffffffffffffff167f8c5be1e5ebec7d5bd14f71427d1e84f3dd0314c0f7b2291e5b200ac8c7c3b925846040518082815260200191505060405180910390a36001905092915050565b60045481565b60008073ffffffffffffffffffffffffffffffffffffffff168373ffffffffffffffffffffffffffffffffffffffff16141561075c57600080fd5b6000821161076957600080fd5b81600560008673ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1681526020019081526020016000205410156107b557600080fd5b600560008473ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1681526020019081526020016000205482600560008673ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002054011161084157600080fd5b600660008573ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002060003373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff168152602001908152602001600020548211156108ca57600080fd5b610913600560008673ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff168152602001908152602001600020548361116a565b600560008673ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1681526020019081526020016000208190555061099f600560008573ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1681526020019081526020016000205483611181565b600560008573ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002081905550610a68600660008673ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002060003373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff168152602001908152602001600020548361116a565b600660008673ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002060003373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff168152602001908152602001600020819055508273ffffffffffffffffffffffffffffffffffffffff168473ffffffffffffffffffffffffffffffffffffffff167fddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef846040518082815260200191505060405180910390a3600190509392505050565b600360009054906101000a900460ff1681565b60056020528060005260406000206000915090505481565b610b8b610c7c565b610b9457600080fd5b600073ffffffffffffffffffffffffffffffffffffffff166000809054906101000a900473ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff167f8be0079c531659141344cd1fd0a4f28419497f9722a3daafe3b4186f6b6457e060405160405180910390a360008060006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff160217905550565b60008060009054906101000a900473ffffffffffffffffffffffffffffffffffffffff16905090565b60008060009054906101000a900473ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff163373ffffffffffffffffffffffffffffffffffffffff1614905090565b60028054600181600116156101000203166002900480601f016020809104026020016040519081016040528092919081815260200182805460018160011615610100020316600290048015610d695780601f10610d3e57610100808354040283529160200191610d69565b820191906000526020600020905b815481529060010190602001808311610d4c57829003601f168201915b505050505081565b60008073ffffffffffffffffffffffffffffffffffffffff168373ffffffffffffffffffffffffffffffffffffffff161415610dac57600080fd5b60008211610db957600080fd5b81600560003373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff168152602001908152602001600020541015610e0557600080fd5b600560008473ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1681526020019081526020016000205482600560008673ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002054011015610e9257600080fd5b6000600560008573ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002054600560003373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002054019050610f60600560003373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff168152602001908152602001600020548461116a565b600560003373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002081905550610fec600560008673ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1681526020019081526020016000205484611181565b600560008673ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff168152602001908152602001600020819055508373ffffffffffffffffffffffffffffffffffffffff163373ffffffffffffffffffffffffffffffffffffffff167fddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef856040518082815260200191505060405180910390a380600560008673ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002054600560003373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002054011461111d57fe5b600191505092915050565b6006602052816000526040600020602052806000526040600020600091509150505481565b611155610c7c565b61115e57600080fd5b611167816111a9565b50565b60008282111561117657fe5b818303905092915050565b60008082840190508381101580156111995750828110155b61119f57fe5b8091505092915050565b600073ffffffffffffffffffffffffffffffffffffffff168173ffffffffffffffffffffffffffffffffffffffff1614156111e357600080fd5b8073ffffffffffffffffffffffffffffffffffffffff166000809054906101000a900473ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff167f8be0079c531659141344cd1fd0a4f28419497f9722a3daafe3b4186f6b6457e060405160405180910390a3806000806101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055505056fea165627a7a72305820ffff004d4506f5120ab6e858040876b1b458ee6fef2fbb48759039adf1ac3ad70029
```



#### Deployed ByteCode Sourcemap

```
2346:2234:0:-;;;;8:9:-1;5:2;;;30:1;27;20:12;5:2;2346:2234:0;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;2395:18;;;:::i;:::-;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;23:1:-1;8:100;33:3;30:1;27:10;8:100;;;99:1;94:3;90:11;84:18;80:1;75:3;71:11;64:39;52:2;49:1;45:10;40:15;;8:100;;;12:14;2395:18:0;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;3646:285;;;;;;13:2:-1;8:3;5:11;2:2;;;29:1;26;19:12;2:2;3646:285:0;;;;;;;;;;;;;;;;;;;;;;;;;;;;:::i;:::-;;;;;;;;;;;;;;;;;;;;;;;2475:26;;;:::i;:::-;;;;;;;;;;;;;;;;;;;3939:638;;;;;;13:2:-1;8:3;5:11;2:2;;;29:1;26;19:12;2:2;3939:638:0;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;:::i;:::-;;;;;;;;;;;;;;;;;;;;;;;2447:21;;;:::i;:::-;;;;;;;;;;;;;;;;;;;;;;;2510:45;;;;;;13:2:-1;8:3;5:11;2:2;;;29:1;26;19:12;2:2;2510:45:0;;;;;;;;;;;;;;;;;;;:::i;:::-;;;;;;;;;;;;;;;;;;;1248:140;;;:::i;:::-;;971:79;;;:::i;:::-;;;;;;;;;;;;;;;;;;;;;;;1144:92;;;:::i;:::-;;;;;;;;;;;;;;;;;;;;;;;2420:20;;;:::i;:::-;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;23:1:-1;8:100;33:3;30:1;27:10;8:100;;;99:1;94:3;90:11;84:18;80:1;75:3;71:11;64:39;52:2;49:1;45:10;40:15;;8:100;;;12:14;2420:20:0;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;3030:608;;;;;;13:2:-1;8:3;5:11;2:2;;;29:1;26;19:12;2:2;3030:608:0;;;;;;;;;;;;;;;;;;;;;;;;;;;;:::i;:::-;;;;;;;;;;;;;;;;;;;;;;;2562:66;;;;;;13:2:-1;8:3;5:11;2:2;;;29:1;26;19:12;2:2;2562:66:0;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;:::i;:::-;;;;;;;;;;;;;;;;;;;1400:109;;;;;;13:2:-1;8:3;5:11;2:2;;;29:1;26;19:12;2:2;1400:109:0;;;;;;;;;;;;;;;;;;;:::i;:::-;;2395:18;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;:::o;3646:285::-;3713:12;3751:1;3741:6;:11;3740:55;;;;3793:1;3758:9;:21;3768:10;3758:21;;;;;;;;;;;;;;;:31;3780:8;3758:31;;;;;;;;;;;;;;;;:36;3740:55;3732:64;;;;;;3841:6;3807:9;:21;3817:10;3807:21;;;;;;;;;;;;;;;:31;3829:8;3807:31;;;;;;;;;;;;;;;:40;;;;3884:8;3863:38;;3872:10;3863:38;;;3894:6;3863:38;;;;;;;;;;;;;;;;;;3919:4;3912:11;;3646:285;;;;:::o;2475:26::-;;;;:::o;3939:638::-;4021:12;4070:1;4055:17;;:3;:17;;;;4046:27;;;;;;4096:1;4087:6;:10;4078:20;;;;;;4139:6;4119:9;:16;4129:5;4119:16;;;;;;;;;;;;;;;;:26;;4110:36;;;;;;4193:9;:14;4203:3;4193:14;;;;;;;;;;;;;;;;4184:6;4167:9;:14;4177:3;4167:14;;;;;;;;;;;;;;;;:23;:40;4158:50;;;;;;4238:9;:16;4248:5;4238:16;;;;;;;;;;;;;;;:28;4255:10;4238:28;;;;;;;;;;;;;;;;4228:6;:38;;4219:48;;;;;;4297:42;4314:9;:16;4324:5;4314:16;;;;;;;;;;;;;;;;4332:6;4297:16;:42::i;:::-;4278:9;:16;4288:5;4278:16;;;;;;;;;;;;;;;:61;;;;4367:40;4384:9;:14;4394:3;4384:14;;;;;;;;;;;;;;;;4400:6;4367:16;:40::i;:::-;4350:9;:14;4360:3;4350:14;;;;;;;;;;;;;;;:57;;;;4449:54;4466:9;:16;4476:5;4466:16;;;;;;;;;;;;;;;:28;4483:10;4466:28;;;;;;;;;;;;;;;;4496:6;4449:16;:54::i;:::-;4418:9;:16;4428:5;4418:16;;;;;;;;;;;;;;;:28;4435:10;4418:28;;;;;;;;;;;;;;;:85;;;;4535:3;4519:28;;4528:5;4519:28;;;4540:6;4519:28;;;;;;;;;;;;;;;;;;4565:4;4558:11;;3939:638;;;;;:::o;2447:21::-;;;;;;;;;;;;;:::o;2510:45::-;;;;;;;;;;;;;;;;;:::o;1248:140::-;1102:9;:7;:9::i;:::-;1094:18;;;;;;1347:1;1310:40;;1331:6;;;;;;;;;;;1310:40;;;;;;;;;;;;1378:1;1361:6;;:19;;;;;;;;;;;;;;;;;;1248:140::o;971:79::-;1009:7;1036:6;;;;;;;;;;;1029:13;;971:79;:::o;1144:92::-;1184:4;1222:6;;;;;;;;;;;1208:20;;:10;:20;;;1201:27;;1144:92;:::o;2420:20::-;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;:::o;3030:608::-;3093:4;3133:1;3118:17;;:3;:17;;;;3110:26;;;;;;3158:1;3149:6;:10;3141:19;;;;;;3205:6;3180:9;:21;3190:10;3180:21;;;;;;;;;;;;;;;;:31;;3172:40;;;;;;3258:9;:14;3268:3;3258:14;;;;;;;;;;;;;;;;3248:6;3231:9;:14;3241:3;3231:14;;;;;;;;;;;;;;;;:23;:41;;3223:50;;;;;;3278:21;3326:9;:14;3336:3;3326:14;;;;;;;;;;;;;;;;3302:9;:21;3312:10;3302:21;;;;;;;;;;;;;;;;:38;3278:62;;3377:47;3394:9;:21;3404:10;3394:21;;;;;;;;;;;;;;;;3417:6;3377:16;:47::i;:::-;3353:9;:21;3363:10;3353:21;;;;;;;;;;;;;;;:71;;;;3452:40;3469:9;:14;3479:3;3469:14;;;;;;;;;;;;;;;;3485:6;3452:16;:40::i;:::-;3435:9;:14;3445:3;3435:14;;;;;;;;;;;;;;;:57;;;;3529:3;3508:33;;3517:10;3508:33;;;3534:6;3508:33;;;;;;;;;;;;;;;;;;3591:16;3575:9;:14;3585:3;3575:14;;;;;;;;;;;;;;;;3553:9;:21;3563:10;3553:21;;;;;;;;;;;;;;;;:36;:54;3546:62;;;;3626:4;3619:11;;;3030:608;;;;:::o;2562:66::-;;;;;;;;;;;;;;;;;;;;;;;;;;:::o;1400:109::-;1102:9;:7;:9::i;:::-;1094:18;;;;;;1473:28;1492:8;1473:18;:28::i;:::-;1400:109;:::o;2071:117::-;2133:7;2161:1;2156;:6;;2149:14;;;;2181:1;2177;:5;2170:12;;2071:117;;;;:::o;2194:143::-;2256:7;2272:9;2288:1;2284;:5;2272:17;;2306:1;2303;:4;;:12;;;;;2314:1;2311;:4;;2303:12;2296:20;;;;2330:1;2323:8;;;2194:143;;;;:::o;1520:187::-;1614:1;1594:22;;:8;:22;;;;1586:31;;;;;;1662:8;1633:38;;1654:6;;;;;;;;;;;1633:38;;;;;;;;;;;;1691:8;1682:6;;:17;;;;;;;;;;;;;;;;;;1520:187;:::o
```



#### Swarm Source

```
bzzr://ffff004d4506f5120ab6e858040876b1b458ee6fef2fbb48759039adf1ac3ad7
```
