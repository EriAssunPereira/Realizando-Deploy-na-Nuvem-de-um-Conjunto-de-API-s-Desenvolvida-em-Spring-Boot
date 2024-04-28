# Realizando-Deploy-na-Nuvem-de-um-Conjunto-de-API-s-Desenvolvida-em-Spring-Boot

Projeto de API em Spring Boot para um sistema de estacionamento. Vamos dividir o projeto em módulos:

### Módulo 1: Configuração do Projeto Spring Boot
- **Criação do projeto** com Spring Initializr, incluindo as dependências para Spring Web, Spring Data JPA, PostgreSQL Driver e Spring Security.
- **Configuração do banco de dados** PostgreSQL no `application.properties`.

### Módulo 2: Desenvolvimento das API's
- **API de Entrada de Veículos**: Endpoint para registrar a entrada de um veículo, armazenando dados como placa, hora de entrada e tipo de veículo.
- **API de Saída de Veículos**: Endpoint para registrar a saída, calculando o tempo de permanência e o valor a ser cobrado.
- **API de Pagamento**: Endpoint para processar o pagamento e liberar a saída do veículo.

### Módulo 3: Segurança com Spring Security
- **Autenticação**: Configuração do Spring Security para proteger os endpoints e gerenciar o acesso através de autenticação básica ou tokens JWT.
- **Autorização**: Definição de roles e permissões para diferentes tipos de usuários (administradores, operadores).

### Módulo 4: Integração com Banco de Dados
- **Modelagem do Banco de Dados**: Criação de entidades JPA para representar veículos, registros de entrada/saída e transações de pagamento.
- **Repositórios**: Implementação de repositórios Spring Data para operações CRUD.

### Módulo 5: Testes e Cobertura
- **Testes Unitários**: Escrita de testes para validar a lógica de negócios das API's.
- **Testes de Integração**: Testes para verificar a integração entre as API's, segurança e banco de dados.
- **Relatórios de Cobertura**: Uso de ferramentas como JaCoCo para gerar relatórios de cobertura de testes.

### Módulo 6: Documentação e Deploy
- **Documentação da API**: Uso do Swagger para documentar todos os endpoints e modelos.
- **Deploy no Heroku**: Configuração do projeto para deploy no Heroku, incluindo Procfile e configurações de ambiente.

### Exemplo Prático: API de Entrada de Veículos
```java
@RestController
@RequestMapping("/veiculos")
public class VeiculoController {

    @Autowired
    private VeiculoService veiculoService;

    @PostMapping("/entrada")
    public ResponseEntity<Veiculo> registrarEntrada(@RequestBody VeiculoDTO veiculoDTO) {
        Veiculo veiculo = veiculoService.registrarEntrada(veiculoDTO);
        return new ResponseEntity<>(veiculo, HttpStatus.CREATED);
    }
}
```

Este é um exemplo simplificado e você precisará expandir com validações, tratamento de exceções e lógica de negócios conforme necessário. 

Aqui está um exemplo de teste unitário para o módulo de entrada de veículos do seu sistema de estacionamento desenvolvido em Spring Boot:

```java
import static org.junit.Assert.assertEquals;
import static org.mockito.Mockito.*;

import org.junit.Before;
import org.junit.Test;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.MockitoAnnotations;

public class VeiculoServiceTest {

    @Mock
    private VeiculoRepository veiculoRepository;

    @InjectMocks
    private VeiculoService veiculoService;

    private Veiculo veiculo;

    @Before
    public void setup() {
        MockitoAnnotations.initMocks(this);
        veiculo = new Veiculo();
        veiculo.setPlaca("ABC1234");
        veiculo.setHoraEntrada(LocalDateTime.now());
        veiculo.setTipoVeiculo(TipoVeiculo.CARRO);
    }

    @Test
    public void testRegistrarEntrada() {
        when(veiculoRepository.save(any(Veiculo.class))).thenReturn(veiculo);
        Veiculo resultado = veiculoService.registrarEntrada(veiculo);
        assertEquals("ABC1234", resultado.getPlaca());
        verify(veiculoRepository, times(1)).save(veiculo);
    }
}
```

Neste exemplo, estamos utilizando o framework Mockito para simular o comportamento do repositório `VeiculoRepository`. O método `testRegistrarEntrada` verifica se o serviço `VeiculoService` está registrando a entrada de um veículo corretamente e se o repositório está sendo chamado uma vez com o veículo correto.

Lembre-se de que os testes unitários são essenciais para garantir que cada parte do seu código funcione como esperado e para facilitar a manutenção e a adição de novas funcionalidades no futuro.
