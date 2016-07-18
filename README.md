# Nagios.NRDP.Client.Net
Nagios.NRDP.Client.Net is a wrapper made for .NET, which works with [NRDP](https://exchange.nagios.org/directory/Addons/Passive-Checks/NRDP--2D-Nagios-Remote-Data-Processor/details) (Nagios Remote Data Processor) API.
<br />
The official overview documentation see [here](http://library.nagios.com/library/products/nagiosxi/documentation/499-nrdp-overview).
<br />
About configuring API for Nagios XI see [here](https://library.nagios.com/library/products/nagiosxi/documentation/673-nagios-xi-passive-monitoring-with-nrdswin).

# Build Status
[![Build Status](https://travis-ci.org/IvAlex1986/Nagios.NRDP.Client.Net.svg?branch=master)](https://travis-ci.org/IvAlex1986/Nagios.NRDP.Client.Net)

# Installation
Installation is performed via NuGet [package](https://www.nuget.org/packages/Nagios.NRDP.Client.Net).
```
PM> Install-Package Nagios.NRDP.Client.Net
```

# Example

Sending CheckData request to Host and Service:
```c#
using Nagios.NRDP.Client.Net.Enums;
using Nagios.NRDP.Client.Net.Models.Request;
using Nagios.NRDP.Client.Net.Models.Response;
using System;
using System.Collections.Generic;

namespace Nagios.NRDP.Client.Net.Example
{
    public class NagiosNrdpExample
    {
        private readonly NagiosNrdpClient _nagiosNrdpClient;

        public NagiosNrdpExample(String apiUrl, String token)
        {
            _nagiosNrdpClient = new NagiosNrdpClient(apiUrl, token);
        }

        public Result SendData()
        {
            var host = new Host("Test")
            {
                HostState = HostState.Up,
                StatusData = "Host works fine"
            };

            var service = new Service("Test", "Test Service")
            {
                ServiceState = ServiceState.Critical,
                StatusData = "Critical Error!",
                CheckDatas = new List<CheckData>
                {
                    new CheckData("Ampers", 10.567)
                    {
                        Dimension = "A",
                        WarningScale = 20,
                        ErrorScale = 40,
                        MinScale = 0,
                        MaxScale = 50
                    },
                    new CheckData("Volts", 220.123)
                    {
                        Dimension = "V",
                        MinScale = 0,
                        MaxScale = 340
                    }
                }
            };

            var result = _nagiosNrdpClient.SubmitCheckData(host, service);

            return result;
        }
    }
}

```

# License
This project is licensed under the [MIT license](https://github.com/IvAlex1986/Nagios.NRDP.Client.Net/blob/master/LICENSE).