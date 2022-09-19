# github-trabalho
sftw3
 ListaDesejosAPI . Modelos ;
usando  Microsoft . AspNetCore . Autorização ;
usando  Microsoft . AspNetCore . Mvc ;
usando  Sistema ;
usando  Sistema . Coleções . Genérico ;
usando  Sistema . IdentityModel . Fichas . Jwt ;
usando  Sistema . Linq ;
usando  Sistema . Segurança . Reivindicações ;

@@ -23,33 +26,39 @@ public DesejoController(contexto do UsuarioContext)
        [ Autorizar ]
        public  IActionResult  AdicionaDesejo ([ FromBody ] Desejo  desejo )
        {


            var  id  =  HttpContext . Usuário . Reivindicações . Primeiro ( x  =>  x . Digite  ==  JwtRegisteredClaimNames . Jti ). Valor ;

            desejo . UsuarioId  =  Converter . ToInt32 ( id );

            _contexto . Desejos . Adicionar ( desejo );
            _contexto . SalvarAlterações ();

            _contexto . SalvarAlterações ();    

            return  CreatedAtAction ( nameof ( RecuperaDesejoPorId ), new { Id  =  desejo . Id }, desejo );

        }



        [ HttpGet ]
        public  IActionResult  RecuperaDesejo ()
        [ Autorizar ]
        public  IActionResult  RecuperaDesejoPorId ()
        {
            return  Ok ( _context . Desejos );
        }
            var  id  =  Converter . ToInt32 ( HttpContext . User . Claims . First ( x  =>  x . Type  ==  JwtRegisteredClaimNames . Jti ). Value );

        [ HttpGet ( " {id} " )]
        public  IActionResult  RecuperaDesejoPorId ( int  id )
        {
            Desejo  desejo  =  _context . Desejos . FirstOrDefault ( desejo  =>  desejo . Id  ==  id );
            if ( desejo  !=  null )
            List < Desejo > desejos = _context  . Desejos . Onde ( x => x . UsuarioId == id ). ParaLista ();     
            if ( desejos  !=  null )
            {
                retornar  Ok ( desejo );
                return  Ok ( desejos );
            }

            return  NotFound ();
        }


        [ HttpPut ( " {id} " )]
        [ Autorizar ]

        public  IActionResult  AlteraDesejoPorid ( int  id , [ FromBody ] Desejo  desejoNovo )
        {
@@ -61,10 +70,11 @@ public IActionResult AlteraDesejoPorid(int id, [FromBody] Desejo desejoNovo)

            desejo . Descricao  =  desejoNovo . Descrição ;          
            _contexto . SalvarAlterações ();
            return  NotFound ();
            return  Sem Conteúdo ();
        }

        [ HttpDelete ( " {id} " )]
        [ Autorizar ]

        public  IActionResult  DeletaDesejo ( int  id ) {

@@ -75,8 +85,7 @@ public IActionResult AlteraDesejoPorid(int id, [FromBody] Desejo desejoNovo)
            }
                _contexto . Remover ( desejo );
                _contexto . SalvarAlterações ();
                return  NotFound ();
                return  Sem Conteúdo ();
        }
    }
}

}
  14 
ListaDesejosAPI/Controllers/UsuarioController.cs
@@ -21,11 +21,19 @@ public UsuarioController(contexto do UsuarioContext)
        [ HttpPost ]
        public  IActionResult  AdicionaUsurario ([ FromBody ] Usuario  usuario )
        {
            Usuario  usuarioExistente  =  _context . Usuários . FirstOrDefault ( x  =>  x . Login  ==  usuario . Login );

            if ( usuarioExistente  ==  null )
            {
                _contexto . Usuários . Adicionar ( usuário );

                _contexto . SalvarAlterações ();

                return  CreatedAtAction ( nameof ( RecuperaUsuarioPorId ), new { Id  =  usuario . Id }, usuario );

            }

            _contexto . Usuários . Adicionar ( usuário );
            _contexto . SalvarAlterações ();
            return  CreatedAtAction ( nameof ( RecuperaUsuarioPorId ), new { Id  =  usuario . Id }, usuario );
            return  Ok ( " Usuário Existente " );
        }

        [ HttpGet ]
  1 
ListaDesejosAPI/Startup.cs
@@ -61,6 +61,7 @@ public void ConfigureServices(IServiceCollection services)
            {
                c . SwaggerDoc ( " v1 " , new  OpenApiInfo { Title  =  " ListaDesejosAPI " , Version  =  " v1 " });
            });

        }
