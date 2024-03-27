def getLatestTags() {
    // Initialize an empty list to store tags
    def tags = []

    // Run git command to fetch tags
    def gitCommand = 'git ls-remote --tags origin'
    def process = sh(script: gitCommand, returnStdout: true).trim()

    // Read the output of the git command
    def outputLines = process.readLines()

    // Extract tag names from the output
    outputLines.each { line ->
        def matcher = line =~ /refs\/tags\/(.+)/
        if (matcher) {
            tags.add(matcher[0][1].trim())
        }
    }

    // Return the list of tags
    return tags
}

pipeline {
    agent any

    stages {
        stage('Fetch Latest Tags') {
            steps {
                script {
                    def latestTags = getLatestTags()
                    echo "Latest tags: ${latestTags}"
                    // Check if there are any new release tags
                    def newReleaseTags = latestTags.findAll { tag -> tag =~ /^v\d+\.\d+(\.\d+)?$/ }
                    if (newReleaseTags) {
                        echo "New release tags found: ${newReleaseTags}"
                        // Trigger the subsequent stages or tasks here
                        // For example:
                        // build newReleaseTags
                    } else {
                        echo "No new release tags found."
                    }
                }
            }
        }
        // Add more stages as needed
    }
}
