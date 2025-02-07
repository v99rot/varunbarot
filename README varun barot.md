# Varun Barot - Cybersecurity Portfolio
[github.com/varunbarot]

## 1. Security Operations Center (SOC) Automation Suite

```python name=soc_automation/alert_handler.py
from datetime import datetime
import json

class AlertHandler:
    def __init__(self):
        self.alert_levels = {
            'critical': 1,
            'high': 2,
            'medium': 3,
            'low': 4
        }
    
    def analyze_alert(self, alert_data):
        """
        Analyzes security alerts and determines response priority
        """
        severity = alert_data.get('severity', 'low')
        source_ip = alert_data.get('source_ip')
        destination_ip = alert_data.get('destination_ip')
        
        # Calculate risk score
        risk_score = self._calculate_risk(alert_data)
        
        return {
            'priority': self.alert_levels[severity],
            'risk_score': risk_score,
            'timestamp': datetime.now().isoformat(),
            'recommendation': self._get_recommendation(risk_score)
        }
    
    def _calculate_risk(self, alert_data):
        # Risk calculation logic
        base_score = 0
        if alert_data.get('repeated_attempts', 0) > 5:
            base_score += 20
        if alert_data.get('malicious_indicators', False):
            base_score += 30
        return base_score

    def _get_recommendation(self, risk_score):
        if risk_score > 80:
            return "Immediate isolation required"
        elif risk_score > 60:
            return "Investigate within 1 hour"
        return "Monitor and log"