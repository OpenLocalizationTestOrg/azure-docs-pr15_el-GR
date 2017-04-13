<properties
   pageTitle="Δημιουργήστε ένα εικονικό δίκτυο χρησιμοποιώντας ένα πρότυπο ARM | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να δημιουργήσετε ένα εικονικό δίκτυο χρησιμοποιώντας ένα πρότυπο ARM | Διαχείριση πόρων."
   services="virtual-network"
   documentationCenter=""
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial"/>

# <a name="create-a-virtual-network-by-using-an-arm-template"></a>Δημιουργήστε ένα εικονικό δίκτυο χρησιμοποιώντας ένα πρότυπο ARM

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnet-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Αυτό το έγγραφο περιγράφει τη δημιουργία ενός VNet χρησιμοποιώντας το μοντέλο ανάπτυξης διαχείρισης πόρων. Μπορείτε επίσης να [δημιουργήσετε ένα εικονικό δίκτυο στο μοντέλο κλασική ανάπτυξης](virtual-networks-create-vnet-classic-pportal.md).

Θα μάθετε πώς μπορείτε να κάνετε λήψη και τροποποίηση και υπάρχοντος προτύπου ARM από GitHub και ανάπτυξη του προτύπου από το GitHub, PowerShell και το Azure CLI.

Εάν αναπτύσσετε απλώς το πρότυπο ARM απευθείας από GitHub, χωρίς αλλαγές, προχωρήστε απευθείας στο [Ανάπτυξη προτύπου από github](#deploy-the-arm-template-by-using-click-to-deploy).

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-arm-template-include](../../includes/virtual-networks-create-vnet-arm-template-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-arm-template-ps-include](../../includes/virtual-networks-create-vnet-arm-template-ps-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-arm-template-cli-include](../../includes/virtual-networks-create-vnet-arm-template-cli-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-arm-template-click-include](../../includes/virtual-networks-create-vnet-arm-template-click-include.md)]