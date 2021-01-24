## Liga e/ou Desliga DEBUG em tempo de execução.


### Altere o app_local

no arquivo `config/app_local.php` mudar a linha do debug para
```php
'debug' => file_exists(TMP.'/debug_on') ? true : false,
```

### Cria a função debug
no arquivo src/Controller/AppController.php, acrescentar a seguinte função:

```php
public function debug()
{
    // se NÃO tem autorização pra ligar e desligar o debug sarta fora.
    /*if ( !Autorizacao::checa()  )
    {
        return $this->redirect( '/' );
    }*/

    // se existe o arquivo desliga, se não liga de novo
    if ( file_exists(TMP . DS . 'debug_on') )
    {
        unlink( TMP . DS . 'debug_on' );
        $this->Flash->success( __('O Debug foi DESLIGADO com sucesso ...') );
    } else
    {
        exec("echo 'mas oe' > ".TMP."/debug_on");
        $this->Flash->success( __('O Debug foi LIGADO com sucesso ...') );
    }

    return $this->redirect( '/' );
}
```
* Colocando a função no `AppController` qualquer outro controlador vai poder ligar/desligar o `debug`.

### Crie uma rota como:
No arquivo config/routes acrescente a linha:

```php
$builder->connect('/debug', ['controller' => 'Painel', 'action'=>'debug'] );
```
* Utilize o `controller` de sua preferência, mas se a função ficou no `AppController`, qualquer outro controller vai executar a função.

### Agora basta acessar

http://dominio.com.br/debug, ele vai ligar e/ou desligar o `debug`.
