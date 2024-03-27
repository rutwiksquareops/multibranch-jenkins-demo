def getLatestTags() {
    // Initialize an empty list to store tags
    def tags = []

    // Run git command to fetch tags
    def gitCommand = 'git ls-remote --tags origin'
    def process = sh(script: gitCommand, returnStdout: true).trim()

    // Read the output of the git command
    def output = process

    // Extract tag names from the output
    output.eachLine { line ->
        def parts = line.split("\t")
        def ref = parts[1]
        if (ref =~ /refs\/tags\/(.+)/) {
            tags.add(RegExp.$1.trim())
        }
    }

    // Return the list of tags
    return tags
}
