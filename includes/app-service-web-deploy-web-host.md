### <a name="app-service-plan"></a>Πρόγραμμα εφαρμογής υπηρεσίας

Δημιουργεί το σχέδιο παροχής υπηρεσιών για τη φιλοξενία της εφαρμογής web. Μπορείτε να παρέχετε το όνομα του σχεδίου μέσω της παραμέτρου **hostingPlanName** . Η θέση του σχεδίου είναι στην ίδια θέση που χρησιμοποιείται για την ομάδα πόρων. Οι παράμετροι **sku** και **workerSize** καθορίζονται στο τιμολόγησης επίπεδο και εργαζόμενου μέγεθος

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('hostingPlanName')]",
      "type": "Microsoft.Web/serverfarms",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "[parameters('sku')]",
        "capacity": "[parameters('workerSize')]"
      },
      "properties": {
        "name": "[parameters('hostingPlanName')]"
      }
    },

