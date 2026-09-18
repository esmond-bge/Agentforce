<script type='text/javascript'>
	function initEmbeddedMessaging() {
		try {
			embeddedservice_bootstrap.settings.language = 'en_US'; // For example, enter 'en' or 'en-US'

			embeddedservice_bootstrap.init(
				'00DS800000ErbEH',
				'Github',
				'https://bordgaisenergyeandu--test.sandbox.my.site.com/ESWGithub1789732266691',
				{
					scrt2URL: 'https://bordgaisenergyeandu--test.sandbox.my.salesforce-scrt.com'
				}
			);
		} catch (err) {
			console.error('Error loading Embedded Messaging: ', err);
		}
	};
</script>
<script type='text/javascript' src='https://bordgaisenergyeandu--test.sandbox.my.site.com/ESWGithub1789732266691/assets/js/bootstrap.min.js' onload='initEmbeddedMessaging()'></script>
