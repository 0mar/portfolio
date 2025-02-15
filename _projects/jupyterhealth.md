---
permalink: /projects/jupyterhealth/
---
# Democratizing data in healthcare with JupyterHealth
Imagine Ada, a 45-year-old professional.
She has been struggling with unexplained fatigue and occasional dizziness. Concerned about potential underlying issues, she consulted her GP, Dr. Rivera, who recommended she monitor her blood sugar levels using a traditional finger-prick glucose meter and keep a food and activity diary.

For weeks, Ada diligently recorded her meals, exercise routines, and glucose levels. However, the manual tracking process was tedious, and the scattered data made it difficult for Dr. Rivera to identify any meaningful patterns. The approach lacked the precision needed to pinpoint the interplay between Sarah’s diet, exercise, and overall health.

Concerned about her health, Ada started using a wearable that tracked her activity, heart rate, and glucose levels. These devices generated real-time, detailed insights about how her body responded to different foods and exercise routines. Ideally, Ada would be able to share this data with Dr. Rivera, so she could analyze trends over time, and make more informed recommendations—something that was impossible with manual logs alone. But practically, there are very few options to safely share this data with her GP, let alone in a format in which it can easily be analysed and cross-compared.

# JupyterHealth
This is where JupyterHealth comes in. Instead of relying on fragmented, manually collected data, JupyterHealth enables secure, real-time access to Ada's wearable and glucose monitoring data. By integrating these insights into her overall medical record, Dr. Rivera can use advanced analytics to uncover patterns that were previously invisible—like a specific combination of meals and exercise that led to her fatigue episodes.

With this data-driven, personalized approach, Ada and Dr. Rivera can move from trial-and-error to precise, evidence-based care—transforming how chronic conditions are monitored and managed.

# JupyterHealth: Revolutionizing Health Data Management

JupyterHealth is a collaborative initiative with many partners, among them the 2i2c (the International Interactive Computing Collaboration), Project Jupyter, and Berkeley Institute for Data Science.

As part of Project Jupyter, I worked on the authentication and authorization protocols from the patient data to JupyterHub, using interoperability standards within medical data sharing.

# Securely sharing health data with SMART on FHIR 

## FHIR
The Fast Healthcare Interoperability Resources ([FHIR](https://hl7.org/fhir/)) standard, developed by Health Level Seven International (HL7), provides a framework for exchanging electronic health records (EHR) in a consistent and secure manner. FHIR defines data formats and resources and an API for sharing healthcare information, facilitating interoperability between diverse health systems.

## The role of SMART

Substitutable Medical Applications, Reusable Technologies ([SMART](https://docs.smarthealthit.org/)) builds upon the FHIR standard by incorporating an authorization layer using OAuth 2.0 and OpenID Connect protocols. This extension ensures that third-party applications can securely access FHIR resources with the necessary permissions, enabling seamless integration of apps into EHR systems while maintaining data privacy and security.
![smart]({{ site.url }}{{ site.baseurl }}/assets/images/smart.png)

## Integration with JupyterHub

JupyterHub, already has support to function as both OAuth client and provider. We extended this support to implement the SMART on FHIR protocols. This provides a workflow for patients to provide consent to their health care professionals to access their data. The data, encrypted at rest, can then be imported and decrypted within the medical environment and be used as input for analytics routines.

![jupyterhealth]({{ site.url }}{{ site.baseurl }}/assets/jupyterhealth.png)
*The authentication flow maintains security while providing seamless access to healthcare data*

This project is open source and available on [GitHub](https://github.com/jupyterhealth). We welcome contributions from developers interested in healthcare security and Jupyter infrastructure.