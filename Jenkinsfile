import groovy.json.JsonSlurper

node{
	def apiToken=''
	environment {
		apiToken     = credentials('pt_api_token')
	}
	stage('prepare check'){
		if(apiToken.equals("")){
			println "Please set Jenkins credentials with pt_api_token"
			retun 
		}
	}
	stage('get updates from pivotal'){
		def base ="https://www.pivotaltracker.com/services/v5/"

		def storiesPathTemplate = "projects/%s/stories"
		def storiesLabelPathTemplate = "projects/%s/stories/%s/labels"
		def storyPathTemplate = "projects/%s/stories/%s"

		def pt_project_id='2186074'
		def pt_state='finished'
		def pt_label=''
		def pt_update_after=''

		//def pt_project_id=env.pt_project_id
		//def pt_state=env.pt_state
		//def pt_label=env.pt_label
		//def pt_update_after=env.pt_update_after

		def storiesPath=String.format(storiesPathTemplate,pt_project_id)	

		def query='?fields=current_state%2Clabels%2Cbranches%2Ctasks&date_format=millis&with_state='+pt_state
		if(!pt_update_after.equals("")){
		    query=query+'&updated_after='+pt_update_after
		}
		if(!pt_label.equals("")){
			query=query+'&with_label='+pt_label
		}

		def pt_url=base + storiesPath + query
		def headers=[maskValue: true, name: 'X-TrackerToken', value: apiToken]

		def response = httpRequest acceptType: 'APPLICATION_JSON', contentType: 'APPLICATION_JSON', customHeaders: [headers], responseHandle: 'LEAVE_OPEN', url: pt_url
	//	println('Status: '+response.status)
	//	println('Response: '+response.content)

		def json = new JsonSlurper().parseText(response.content)
		def storyIds, labels, branches
		storyIds=json*.id
		println "storyIds: "+ storyIds
		labels=json*.labels.name
		println "labels: "+labels
		branches=json*.branches.name
		println "branches: "+branches	
	}
}
