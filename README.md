                  # Preencher-campo-no-formulario-automaticamente-PHP
                  
index.html
--------------------------------------------------------------------------------------------------------------
<script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
<script> 
function teste(id){
	var $nome = $("input[name='nome']");
	var $sobrenome = $("input[name='sobrenome']");
	$.getJSON('consulta.php',{ 
	id_pessoa: id
	},function(json){
	$nome.val(json.nome);
	$sobrenome.val( json.sobrenome );
	});	
}
</script> 

<pre style="text-align: center;">
	<h3>1- Preenchimento de campos- select</h3>

<form method="POST" action="">
	<label>ID Aluno</label>
	<select  id="id_pessoa" onchange="teste(this.value);">
		<option value="1">ID 1</option>
	    <option value="2">ID 2</option>
	    <option value="3">ID 3</option>
	    <option value="4">ID 4</option>
	</select>

	<label>Nome_Aluno</label>
	<input type="text" class="form-control" name="nome">
			
    <label>sobrenome</label>
	<input type="text" class="form-control" name="sobrenome">
</form>

	<h3>2- Preenchimento de campos- input</h3>
	<label>ID Aluno</label>
	<input  id="id_pessoa" onchange="teste(this.value);">
</pre>

consulta.php
---------------------------------------------------------------------------------------------
<?php
if(isset($_GET['id_pessoa'])){

	$id_pessoa=$_GET['id_pessoa'];

  	$PDO = new PDO('mysql:host=localhost;dbname=projeto_tcc', 'root', '');
  	$PDO->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);

    $RESULT = $PDO->query("SELECT * FROM pessoa WHERE id_pessoa = '$id_pessoa' LIMIT 1");
 	$cont=0;
    foreach ($RESULT->fetchAll() as $key => $value) {	
		$valor['nome']      = $value['nome'];
		$valor['sobrenome'] = $value['sobrenome'];
		$cont=1;
	}

	if ($cont==0) {
		$valor['nome']      = 'Nome não encontrado';
		$valor['sobrenome'] = 'Sobrenome não encontrado';
	}
	
	echo json_encode($valor);
}
?>

