<cfcomponent name="PostmarkSpamcheckAPI" hint="Wrapper for the Postmark Spam Score API">

	<cfset PostmarkSpamcheckUrl = "http://spamcheck.postmarkapp.com/filter">
	

	<cffunction name="getScore" access="public" returntype="string" description="Returns the score for the spam check">
		<cfargument name="email" required="true" type="string" displayname="The raw dump of the email to be filtered, including all headers">

		<cfreturn trim(doSpamCheck(arguments.email, "short").score)>
	</cffunction>
	
	<cffunction name="getReport" access="public" returntype="string" description="Returns the detailed SpamAssassin report for the spam check">
		<cfargument name="email" required="true" type="string" displayname="The raw dump of the email to be filtered, including all headers">
		
		<cfreturn doSpamCheck(arguments.email, "long").report>
	</cffunction>
	
	
	
		
	<cffunction name="doSpamCheck" access="public" returntype="struct" description="Returns the full JSON response of the spam check as a struct">
		<cfargument name="email" required="true" type="string" displayname="The raw dump of the email to be filtered, including all headers">
		<cfargument name="options" required="true" type="string" displayname="Must either be 'long' for a full report of processing, or 'short' for a score request">
		
		<cfset var postData = "">
		<cfset var result = "">
			
		<!--- Assemble data to send to Spam Score API --->
		<cfset postData = StructNew()>
		<cfset postData["email"] = arguments.email>
		<cfset postData["options"] = arguments.options>
		
		<!--- Send the request to Spam Score API --->
		<cftry>
			<cfhttp url="#PostmarkSpamcheckUrl#" method="post" timeout="15">
				<cfhttpparam type="header" name="Accept" value="application/json">
				<cfhttpparam type="header" name="Content-type" value="application/json">
				<cfhttpparam type="body" encoded="no" value="#SerializeJSON(postData)#">
			</cfhttp>
			<cfcatch>
				<cfthrow message="Unable to connect to Postmark Spam Score API">
			</cfcatch>
		</cftry>
		
		<!--- Did we get a valid HTTP response? --->
		<cfif cfhttp.StatusCode is not "200 OK">
			<cfthrow message="Error from Postmark Spam Score API: #cfhttp.StatusCode#">
		</cfif>
		
		<!--- We should now have a valid JSON response, but double-check... --->
		<cfif not isJson(cfhttp.filecontent)>
			<cfthrow message="Data returned from Postmark Spam Score API is not valid JSON data">
		</cfif>
		
		<!--- Convert the JSON data to a struct --->
		<cfset result = deserializeJson(cfhttp.filecontent)>
		
		<!--- Did the spam check report an error (probably due to no email content being passed in) --->
		<cfif not result.success>
			<cfthrow message="Error from Postmark Spam Score API: #result.message#">
		</cfif>		
		
		<cfreturn result>
	</cffunction>
	
</cfcomponent>