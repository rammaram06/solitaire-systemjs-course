stage 'CI'
node('win2012r2') {
    checkout scm

  //git branch: 'jenkins2-course', url: 'https://github.com/rammaram06/solitaire-systemjs-course.git'
    // pull dependencies from npm
    // on windows use: bat 'npm install'
    //sh 'npm install'

    bat 'npm install'

    // stash code & dependencies to expedite subsequent testing
    // and ensure same code & dependencies are used throughout the pipeline
    // stash is a temporary archive
    stash name: 'everything', 
          excludes: 'test-results/**', 
          includes: '**'
    
    // test with PhantomJS for "fast" "generic" results
    // on windows use: bat 'npm run test-single-run -- --browsers PhantomJS'
    //sh 'npm run test-single-run -- --browsers PhantomJS'
    bat 'npm run test-single-run -- --browsers PhantomJS'
    
    // archive karma test results (karma is configured to export junit xml files)
   junit 'test-results/**/test-results.xml'
          
}
    
    stage 'Browser Testing'
    parallel chrome: {
        runTests("Chrome")
    }, firefox: {
        runTests("Firefox")
    }
    

def runTests(browser) {
    node('win2012r2') {
        bat 'del *.* /F /Q'
        unstash 'everything'
        bat "npm run test-single-run -- --browsers ${browser}"
        junit 'test-results/**/test-results.xml'

    }
}

//input 'Deploy to staging?'

//stage name: 'Deploy', concurrency: 1
 // node('win2012r2') {
   //   bat "echo '<h1>${env.BUILD_DISPLAY_NAME}<h1>' >> app/index.html"
     // bat 'docker-compose up -d --build'
  //}
