<script type='text/javascript'>
	function initEmbeddedMessaging() {
		try {
			embeddedservice_bootstrap.settings.language = 'en_US'; // For example, enter 'en' or 'en-US'

			embeddedservice_bootstrap.init(
				'00DRR00000V4G17',
				'MIAWPOC_Github',
				'https://bordgaisenergyeandu--afdev2.sandbox.my.site.com/ESWMIAWPOCGithub1793617406174',
				{
					scrt2URL: 'https://bordgaisenergyeandu--afdev2.sandbox.my.salesforce-scrt.com'
				}
			);
		} catch (err) {
			console.error('Error loading Embedded Messaging: ', err);
		}
	};
</script>
<script type='text/javascript' src='https://bordgaisenergyeandu--afdev2.sandbox.my.site.com/ESWMIAWPOCGithub1793617406174/assets/js/bootstrap.min.js' onload='initEmbeddedMessaging()'></script>
