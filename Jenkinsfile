// Plantilla de PIPELINE.
// Autor: IvanciniGT

// Al cambiar la verisón sólo se recarga la configuración del proyecto... pero no se ejecutarán las tareas
VERSION_DEL_PIPELINE="0"

// Añadir aquí los parámetros del pipeline
PARAMETROS_DE_MI_PIPELINE=[
    booleanParam (defaultValue: true, description: 'Indica si se genera un error', name: 'GENERAR_ERROR'),
    string(defaultValue: '5', description: 'Tiempo que simulamos que tarda la tarea', name: 'DEMORA'),
    string(defaultValue: 'Contenido por defecto', description: 'Contenido del fichero que generamos', name: 'CONTENIDO'),
    string(defaultValue: '', description: 'Fichero que generamos', name: 'FICHERO'),
    // choice(choices: ['Valor1', 'Valor2', 'Valor3'], description: 'Descripción de mi parámetro', name: 'MI_PARAM_LISTA')
]

// Añadir aquí los parámetros del pipeline
TRIGGERS_DE_MI_PIPELINE=[
    //cron("* * * * *"),                                                        // Ejecutar cada minuto
    //pollSCM("* * * * *")                                                      // Ejecutar cuando cambie algo en GIT
    //upstream(upstreamProjects: 'PROYECTO_DISPARADOR', threshold: 'FAILURE')   // Ejecutar tras otro proyecto (Incluso si falla)
]

// No tocar este trozo... es el que hace la magia
if (this.params.getOrDefault('VERSION_DEL_PIPELINE',"-1")!=VERSION_DEL_PIPELINE){
    stage('Configuración del JOB'){
        creoConfiguracion()
    }
    return
}
// Aquí empiezan las tareas propias de mi pipeline
node {
    // Recordar hacer checkout del proyetco si es necesario. 
    // Con esta sintaxis no se hace por defecto
        // checkout scm     // En este caso se hace un checkout del mismo repo donde está el JENKINSFILE
        // checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'MiCredencialGitHub', url: 'https://github.com/IvanciniGT/cursoJenkinsWebapp.git']]])
    try{
        stage ("Hacer las cosas") {
            echo "Hago mis cosas de mi tarea"
            sh "sleep ${DEMORA}"
        }
        stage ("Hacer cosas peligrosas") {
            echo "Hago mis cosas peligrosas"
            try{
                if (this.params.GENERAR_ERROR){
                    sh "exit 1"
                }
            }catch(exc){
                echo 'Upsss, pues si ha sido peligrosa !!!!'
                throw exc
            }
            echo 'Pues no ha sido para tanto !!!!'
        }
        stage ("Generar fichero") {
            sh "echo '${CONTENIDO}' > ${FICHERO}"
        }
        stage ("Guardar fichero") {
            archiveArtifacts(artifacts: '${FICHERO}', followSymlinks: false)
            echo 'Fichero guardado'
        }
    }
    finally {
        //stage ("Limpieza del workspace") {
        //    cleanWs()
        //}
    }
}












// Función que crea la configuración. NO TOCAR si no sabes lo que estás haciendo ;)
def creoConfiguracion(){
    properties(
        [
            parameters(
                [
                    choice(name: 'VERSION_DEL_PIPELINE', description: 'Version del pipeline instalada', choices: [ VERSION_DEL_PIPELINE ])
                ] + PARAMETROS_DE_MI_PIPELINE
            ),
            pipelineTriggers( TRIGGERS_DE_MI_PIPELINE )
        ]
    )
}
 