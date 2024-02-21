node {
    docker.image('node:16-buster-slim').inside('-p 3000:3000') {
        stage('Build') {
            sh 'npm install'
        }

        stage('Test') {
            sh './jenkins/scripts/test.sh'
        }

        stage('Manual Approval') {
        // Menampilkan pesan input dan menunggu persetujuan
        def userInput = input(id: 'manual-approval', message: 'Lanjutkan ke tahap Deploy?', parameters: [choice(choices: 'Proceed\nAbort', description: 'Pilih tindakan', name: 'Action')])

        // Memeriksa tindakan yang dipilih
        if (userInput == 'Proceed') {
            echo 'Melanjutkan ke tahap Deploy...'
        } else {
            echo 'Menghentikan eksekusi pipeline.'
            currentBuild.result = 'ABORTED'
            error('Pipeline dihentikan oleh pengguna.')
        }
        }

        stage('Deploy') {
            sh './jenkins/scripts/deliver.sh'
            sleep(60)
            //input message: 'Sudah selesai menggunakan React App? (Klik "Proceed" untuk mengakhiri)'
            sh './jenkins/scripts/kill.sh'
        }
    }
}
