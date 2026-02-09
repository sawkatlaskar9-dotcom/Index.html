<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Legal Location Tracker</title>
</head>
<body>
    <h1>Consent-Based Location Tracker</h1>
    <p>Enter your phone number and grant location permission to share your location (for demo purposes only).</p>
    <input type="tel" id="phone" placeholder="Phone Number">
    <button onclick="trackLocation()">Track My Location</button>
    <div id="location"></div>

    <script>
        function trackLocation() {
            const phone = document.getElementById('phone').value;
            if (!phone) {
                alert('Please enter a phone number.');
                return;
            }

            // Check for geolocation support
            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition(
                    (position) => {
                        const lat = position.coords.latitude;
                        const lon = position.coords.longitude;
                        document.getElementById('location').innerHTML = 
                            `Phone: ${phone}<br>Latitude: ${lat}<br>Longitude: ${lon}`;
                        
                        // Send to backend (replace with your API endpoint)
                        fetch('/api/track', {
                            method: 'POST',
                            headers: { 'Content-Type': 'application/json' },
                            body: JSON.stringify({ phone, lat, lon })
                        });
                    },
                    (error) => {
                        alert('Location access denied or unavailable. Consent is required.');
                    }
                );
            } else {
                alert('Geolocation is not supported by this browser.');
            }
        }
    </script>
</body>
</html>
