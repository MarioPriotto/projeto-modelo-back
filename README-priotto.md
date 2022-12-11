# Server

# Collections: Logs, Tasks, Users

# deixar rodando o MongoDB Compass (localhost:27017)

# node index.js ---> roda em stand by (http://localhost:8080)

# config :::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

==>>cloudinary.config.js (95)
....configura com site que armazenará as imagens (fotos) pasta: enap-92

==>>db.config.js (96)
....configura acesso ao BD mongoDB - variável .env MONGODB_URI

==>>jwt.config.js (97)
....configura payload, signature, expiração do TOKEN

# middlewares ::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

==>>attachCurrentUser.js (98)
....middleare checa se existe o usuario de requ.auth (payload de headers)
....checa se o usuário tem o e-mail confirmado
....faz com que req.current.user = user (todos os dados, menos senha, do BD)

==>>isAdmin.js (99)
....middleare checa se o usuario é ADMIN

==>>isAuth.js (100)
....middleare cria a chave req.auth na requisição (com payload)

# model ::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

==>>log.model.js (101)
....model define o SCHEMA para log (user,task,date,status)

==>>task.model.js (102)
....model define o SCHEMA para task (details,complete,dateFin,user,collab,status)

==>>user.model.js (103)
....model define o SCHEMA para user:
....(name,age,email,role,active,tasks[],passwordHash,profilePic,confirmEmail)

# routes :::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

==>>log.routes.js (104)
....responde pela rota "/my-logs" (isAuth,attachCurrentUser)
....retorna todos os logs do BD para o usuário de req.currenUser.\_id
....(name,age,email,role,active,tasks[],passwordHash,profilePic,confirmEmail)

==>>task.routes.js (105)
....responde pela rota post "/create-task" (isAuth,attachCurrentUser)
....responde pela rota get "/my-task" (isAuth,attachCurrentUser)
....responde pela rota put "/edit/:idTask" (isAuth,attachCurrentUser)
....responde pela rota delete "/delete/:idTask" (isAuth,attachCurrentUser)
....responde pela rota put "/complete/:idTask" (isAuth,attachCurrentUser)
....responde pela rota get "/all-tasks"

==>>uploadImage.routes.js (108)
....responde pela rota post "/upload" (uploadImg.single("picture"))
....retorna o path da imagem carregada (para poder gravar a string no BD)

==>>user.routes.js (109)
....responde pela rota post "/sign-up"
....responde pela rota get "/activate-account/:idUser"
....responde pela rota post "/login"
....responde pela rota get "/profile" (isAuth,attachCurrentUser)
....responde pela rota get "/all-users" (isAuth,isAdmin,attachCurrentUser)
....responde pela rota put "/edit" (isAuth,attachCurrentUser)
....responde pela rota delete "/delete" (isAuth,attachCurrentUser)
....responde pela rota get "/oneUser/:id"

# / ::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

==>>index.js (114)
....habilita variáveis de memória
....instancia variável do express
....ajusta REACT_URL para cors
....habilita o express para JSON
....conecta o banco de dados
....seta cada uma das rotas:
..../user
..../task
..../uploadImage
..../log
....ativa o listen na porta configurada em .env
