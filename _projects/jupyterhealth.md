---
permalink: /projects/jupyterhealth/
---

# Securing Healthcare Data: SMART on FHIR Authentication for JupyterHealth

Healthcare data needs robust security while remaining accessible to researchers and clinicians. This balance becomes even more critical as data science tools become essential for medical research and patient care.

## The problem
Traditional OAuth systems weren't designed with healthcare's strict security requirements in mind. When researchers need to access patient data through Jupyter notebooks, they need seamless authentication that maintains HIPAA compliance and respects patient privacy.

## The solution
We enhanced JupyterHub's authentication system to fully support SMART on FHIR - the healthcare industry's standard for secure API access. The system allows researchers to:
- Access patient data directly in notebooks
- Maintain continuous authentication during long analyses
- Work within EHR systems' security boundaries

![jupyterhealth]({{ site.url }}{{ site.baseurl }}/assets/jupyterhealth.png)
*The authentication flow maintains security while providing seamless access to healthcare data*

## Technical Implementation
The core of our work involved extending JupyterHub's OAuthenticator:

```python
class SMARTonFHIRAuthenticator(OAuthenticator):
    def __init__(self, **kwargs):
        super().__init__(**kwargs)
        self.headers = {"Accept": "application/fhir+json"}
        
    async def authenticate(self, handler, data=None):
        # Handle SMART launch context
        launch_context = handler.get_argument('launch', None)
        if launch_context:
            self.launch_state = launch_context
            
        # Proceed with OAuth flow
        user = await super().authenticate(handler, data)
        
        # Verify SMART-specific scopes
        if not self._verify_smart_scopes(user.auth_state):
            return None
            
        return user
```

This implementation handles:
1. SMART launch contexts for EHR integration
2. Healthcare-specific OAuth scopes
3. Refresh token management for long sessions

## Deployment and Impact
My focus was on supporting secure authentication and authorization by implementing the [SMART](https://docs.smarthealthit.org/) on [FHIR](https://hl7.org/fhir/) protocol in JupyterHub.

![smart]({{ site.url }}{{ site.baseurl }}/assets/images/smart.png)

## Future Work
We're expanding the system to support:
- Additional FHIR resources and operations
- Bulk data access protocols
- Advanced audit logging

Our code is open source and available on GitHub. We welcome contributions from developers interested in healthcare security and Jupyter infrastructure.

*This work is part of JupyterHealth's mission to create secure, accessible platforms for healthcare research.*