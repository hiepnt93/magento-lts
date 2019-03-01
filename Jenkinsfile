node {
 	// Clean workspace before doing anything
    deleteDir()

    MAGENTO_DIR='magento'

    try {
        stage ('Clone') {
        	checkout scm
        }
        
        branchInfo = getBranchInfo()
        echo brandInfo.type
	echo brandInfo.version
    } catch (err) {
        currentBuild.result = 'FAILED'
        // Send email or another notification
        throw err
    }
}

def getBranchInfo() {
    def branchInfo = [:]
    branchData = BRANCH_NAME.split('/')
    if (branchData.size() == 2) {
        branchInfo['type'] = branchData[0]
        branchInfo['version'] = branchData[1]
    } else {
        branchInfo['type'] = BRANCH_NAME
        branchInfo['version'] = BRANCH_NAME
    }
    return branchInfo
}

def confirmServerToDeploy() {
    def server = false
    try {
        timeout(time:2, unit:'HOURS') {
            server = input(
                id: 'environmentInput', message: 'Deployment Settings', parameters: [
                choice(choices: "stage\nproduction\nboth", description: 'Target server to deploy', name: 'deployServer')
            ])
        }
    } catch (err) {
        echo "Timeout expired. Environment was not set by user"
    }
    return server
}
