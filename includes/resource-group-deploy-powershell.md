## <a name="how-to-deploy-with-powershell"></a>Πώς να αναπτύξετε με το PowerShell

1. Συνδεθείτε στο λογαριασμό σας Azure.

          Add-AzureAccount

   Μετά την παροχή τα διαπιστευτήριά σας, η εντολή επιστρέφει πληροφορίες σχετικά με το λογαριασμό σας.

          Id                             Type       ...
          --                             ----    
          example@contoso.com            User       ...   

2. Εάν έχετε πολλές συνδρομές, δώστε το αναγνωριστικό συνδρομής που θέλετε να χρησιμοποιήσετε για ανάπτυξη. 

          Select-AzureSubscription -SubscriptionID <YourSubscriptionId>

3. Μετάβαση στη λειτουργική μονάδα Azure από διαχειριστή πόρων.

          Switch-AzureMode AzureResourceManager

4. Εάν δεν έχετε μια υπάρχουσα ομάδα πόρων, δημιουργήστε μια νέα ομάδα πόρων. Εισαγάγετε το όνομα της ομάδας πόρων και θέση που χρειάζεστε για τη λύση.

        New-AzureResourceGroup -Name ExampleResourceGroup -Location "West US"

   Επιστρέφεται μια σύνοψη της νέας ομάδας πόρων.

        ResourceGroupName : ExampleResourceGroup
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                    Actions  NotActions
                    =======  ==========
                    *
        ResourceId        : /subscriptions/######/resourceGroups/ExampleResourceGroup

5. Για να δημιουργήσετε μια νέα ανάπτυξη για την ομάδα πόρων, εκτελέστε την εντολή **Δημιουργία AzureResourceGroupDeployment** και δώστε τις απαραίτητες παραμέτρους. Οι παράμετροι θα περιλαμβάνει ένα όνομα για την ανάπτυξη, το όνομα του σας ομάδα πόρων, η διαδρομή ή διεύθυνση URL στο πρότυπο που δημιουργήσατε και τυχόν άλλες παραμέτρους που απαιτούνται για το σενάριό σας. 
   
   Έχετε τις ακόλουθες επιλογές για την παροχή τιμές παραμέτρων: 
   
   - Χρησιμοποιήστε το ενσωματωμένο παραμέτρους.

            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -myParameterName "parameterValue"

   - Χρησιμοποιήστε ένα αντικείμενο παραμέτρου.

            $parameters = @{"<ParameterName>"="<Parameter Value>"}
            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -TemplateParameterObject $parameters

   - Χρησιμοποιώντας ένα αρχείο παραμέτρων.

            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -TemplateParameterFile <PathOrLinkToParameterFile>

  Όταν έχει αναπτυχθεί την ομάδα των πόρων, θα δείτε μια σύνοψη των ανάπτυξης.

             DeploymentName    : ExampleDeployment
             ResourceGroupName : ExampleResourceGroup
             ProvisioningState : Succeeded
             Timestamp         : 4/14/2015 7:00:27 PM
             Mode              : Incremental
             ...

6. Για να λάβετε πληροφορίες σχετικά με τα σφάλματα ανάπτυξης.

        Get-AzureResourceGroupLog -ResourceGroup ExampleResourceGroup -Status Failed

7. Για λεπτομερείς πληροφορίες σχετικά με τα σφάλματα ανάπτυξης.

        Get-AzureResourceGroupLog -ResourceGroup ExampleResourceGroup -Status Failed -DetailedOutput
