pipeline {
    agent any
    stages {
        stage ("info") {
            when {
		        changeRequest()
			}
			steps {
				powershell 'gci env:\\ | ft name,value -autosize'
				
                // add a ref to git config to make it aware of master
                powershell '& git config --add remote.origin.fetch +refs/heads/master:refs/remotes/origin/master'
				
                // now fetch master so you can do a diff against it 
                powershell '& git fetch --no-tags'
				
                // do the diff and set some variable based on the result
                powershell '''
					$DiffToMaster = & git diff --name-only origin/master..origin/$env:BRANCH_NAME
					Switch ($DiffToMaster) {
						'server-1607/base.json' {$env:PACK_BASE = $true}
						'server-1607/basic.json' {$env:PACK_BASIC = $true}
						'server-1607/algo.json' {$env:PACK_ALGO = $true}
						'server-1607/build.json' {$env:PACK_BUILD = $true}
						'server-1607/calc.json' {$env:PACK_CALC = $true}
					}
					gci env:/PACK_*
				'''
			}
		}
  }
}
