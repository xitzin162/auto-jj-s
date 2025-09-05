-- AUTO J/'S - GLHUB Script para enviar no chat (CORRIGIDO E ACELERADO)
-- Cole este script em StarterPlayerScripts

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local StarterGui = game:GetService("StarterGui")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Variáveis de controle
local isRunning = false
local currentNumber = 0
local config = {
    comecarDe = 0,
    fazerAte = 500,  -- Alterado para 500
    finalPrefix = 1,
    pular = true,
    velocidade = 0.5 -- NOVO: Controle de velocidade (segundos entre mensagens)
}

-- Tabela de números por extenso (expandida até 500)
local numerosExtenso = {
    [0] = "ZERO", [1] = "UM", [2] = "DOIS", [3] = "TRÊS", [4] = "QUATRO",
    [5] = "CINCO", [6] = "SEIS", [7] = "SETE", [8] = "OITO", [9] = "NOVE",
    [10] = "DEZ", [11] = "ONZE", [12] = "DOZE", [13] = "TREZE", [14] = "QUATORZE",
    [15] = "QUINZE", [16] = "DEZESSEIS", [17] = "DEZESSETE", [18] = "DEZOITO", [19] = "DEZENOVE",
    [20] = "VINTE", [21] = "VINTE E UM", [22] = "VINTE E DOIS", [23] = "VINTE E TRÊS", [24] = "VINTE E QUATRO",
    [25] = "VINTE E CINCO", [26] = "VINTE E SEIS", [27] = "VINTE E SETE", [28] = "VINTE E OITO", [29] = "VINTE E NOVE",
    [30] = "TRINTA", [31] = "TRINTA E UM", [32] = "TRINTA E DOIS", [33] = "TRINTA E TRÊS", [34] = "TRINTA E QUATRO",
    [35] = "TRINTA E CINCO", [36] = "TRINTA E SEIS", [37] = "TRINTA E SETE", [38] = "TRINTA E OITO", [39] = "TRINTA E NOVE",
    [40] = "QUARENTA", [41] = "QUARENTA E UM", [42] = "QUARENTA E DOIS", [43] = "QUARENTA E TRÊS", [44] = "QUARENTA E QUATRO",
    [45] = "QUARENTA E CINCO", [46] = "QUARENTA E SEIS", [47] = "QUARENTA E SETE", [48] = "QUARENTA E OITO", [49] = "QUARENTA E NOVE",
    [50] = "CINQUENTA", [51] = "CINQUENTA E UM", [52] = "CINQUENTA E DOIS", [53] = "CINQUENTA E TRÊS", [54] = "CINQUENTA E QUATRO",
    [55] = "CINQUENTA E CINCO", [56] = "CINQUENTA E SEIS", [57] = "CINQUENTA E SETE", [58] = "CINQUENTA E OITO", [59] = "CINQUENTA E NOVE",
    [60] = "SESSENTA", [61] = "SESSENTA E UM", [62] = "SESSENTA E DOIS", [63] = "SESSENTA E TRÊS", [64] = "SESSENTA E QUATRO",
    [65] = "SESSENTA E CINCO", [66] = "SESSENTA E SEIS", [67] = "SESSENTA E SETE", [68] = "SESSENTA E OITO", [69] = "SESSENTA E NOVE",
    [70] = "SETENTA", [71] = "SETENTA E UM", [72] = "SETENTA E DOIS", [73] = "SETENTA E TRÊS", [74] = "SETENTA E QUATRO",
    [75] = "SETENTA E CINCO", [76] = "SETENTA E SEIS", [77] = "SETENTA E SETE", [78] = "SETENTA E OITO", [79] = "SETENTA E NOVE",
    [80] = "OITENTA", [81] = "OITENTA E UM", [82] = "OITENTA E DOIS", [83] = "OITENTA E TRÊS", [84] = "OITENTA E QUATRO",
    [85] = "OITENTA E CINCO", [86] = "OITENTA E SEIS", [87] = "OITENTA E SETE", [88] = "OITENTA E OITO", [89] = "OITENTA E NOVE",
    [90] = "NOVENTA", [91] = "NOVENTA E UM", [92] = "NOVENTA E DOIS", [93] = "NOVENTA E TRÊS", [94] = "NOVENTA E QUATRO",
    [95] = "NOVENTA E CINCO", [96] = "NOVENTA E SEIS", [97] = "NOVENTA E SETE", [98] = "NOVENTA E OITO", [99] = "NOVENTA E NOVE",
    [100] = "CEM", [101] = "CENTO E UM", [102] = "CENTO E DOIS", [103] = "CENTO E TRÊS", [104] = "CENTO E QUATRO",
    [105] = "CENTO E CINCO", [106] = "CENTO E SEIS", [107] = "CENTO E SETE", [108] = "CENTO E OITO", [109] = "CENTO E NOVE",
    [110] = "CENTO E DEZ", [111] = "CENTO E ONZE", [112] = "CENTO E DOZE", [113] = "CENTO E TREZE", [114] = "CENTO E QUATORZE",
    [115] = "CENTO E QUINZE", [116] = "CENTO E DEZESSEIS", [117] = "CENTO E DEZESSETE", [118] = "CENTO E DEZOITO", [119] = "CENTO E DEZENOVE",
    [120] = "CENTO E VINTE", [121] = "CENTO E VINTE E UM", [122] = "CENTO E VINTE E DOIS", [123] = "CENTO E VINTE E TRÊS", [124] = "CENTO E VINTE E QUATRO",
    [125] = "CENTO E VINTE E CINCO", [126] = "CENTO E VINTE E SEIS", [127] = "CENTO E VINTE E SETE", [128] = "CENTO E VINTE E OITO", [129] = "CENTO E VINTE E NOVE",
    [130] = "CENTO E TRINTA", [131] = "CENTO E TRINTA E UM", [132] = "CENTO E TRINTA E DOIS", [133] = "CENTO E TRINTA E TRÊS", [134] = "CENTO E TRINTA E QUATRO",
    [135] = "CENTO E TRINTA E CINCO", [136] = "CENTO E TRINTA E SEIS", [137] = "CENTO E TRINTA E SETE", [138] = "CENTO E TRINTA E OITO", [139] = "CENTO E TRINTA E NOVE",
    [140] = "CENTO E QUARENTA", [141] = "CENTO E QUARENTA E UM", [142] = "CENTO E QUARENTA E DOIS", [143] = "CENTO E QUARENTA E TRÊS", [144] = "CENTO E QUARENTA E QUATRO",
    [145] = "CENTO E QUARENTA E CINCO", [146] = "CENTO E QUARENTA E SEIS", [147] = "CENTO E QUARENTA E SETE", [148] = "CENTO E QUARENTA E OITO", [149] = "CENTO E QUARENTA E NOVE",
    [150] = "CENTO E CINQUENTA", [151] = "CENTO E CINQUENTA E UM", [152] = "CENTO E CINQUENTA E DOIS", [153] = "CENTO E CINQUENTA E TRÊS", [154] = "CENTO E CINQUENTA E QUATRO",
    [155] = "CENTO E CINQUENTA E CINCO", [156] = "CENTO E CINQUENTA E SEIS", [157] = "CENTO E CINQUENTA E SETE", [158] = "CENTO E CINQUENTA E OITO", [159] = "CENTO E CINQUENTA E NOVE",
    [160] = "CENTO E SESSENTA", [161] = "CENTO E SESSENTA E UM", [162] = "CENTO E SESSENTA E DOIS", [163] = "CENTO E SESSENTA E TRÊS", [164] = "CENTO E SESSENTA E QUATRO",
    [165] = "CENTO E SESSENTA E CINCO", [166] = "CENTO E SESSENTA E SEIS", [167] = "CENTO E SESSENTA E SETE", [168] = "CENTO E SESSENTA E OITO", [169] = "CENTO E SESSENTA E NOVE",
    [170] = "CENTO E SETENTA", [171] = "CENTO E SETENTA E UM", [172] = "CENTO E SETENTA E DOIS", [173] = "CENTO E SETENTA E TRÊS", [174] = "CENTO E SETENTA E QUATRO",
    [175] = "CENTO E SETENTA E CINCO", [176] = "CENTO E SETENTA E SEIS", [177] = "CENTO E SETENTA E SETE", [178] = "CENTO E SETENTA E OITO", [179] = "CENTO E SETENTA E NOVE",
    [180] = "CENTO E OITENTA", [181] = "CENTO E OITENTA E UM", [182] = "CENTO E OITENTA E DOIS", [183] = "CENTO E OITENTA E TRÊS", [184] = "CENTO E OITENTA E QUATRO",
    [185] = "CENTO E OITENTA E CINCO", [186] = "CENTO E OITENTA E SEIS", [187] = "CENTO E OITENTA E SETE", [188] = "CENTO E OITENTA E OITO", [189] = "CENTO E OITENTA E NOVE",
    [190] = "CENTO E NOVENTA", [191] = "CENTO E NOVENTA E UM", [192] = "CENTO E NOVENTA E DOIS", [193] = "CENTO E NOVENTA E TRÊS", [194] = "CENTO E NOVENTA E QUATRO",
    [195] = "CENTO E NOVENTA E CINCO", [196] = "CENTO E NOVENTA E SEIS", [197] = "CENTO E NOVENTA E SETE", [198] = "CENTO E NOVENTA E OITO", [199] = "CENTO E NOVENTA E NOVE",
    [200] = "DUZENTOS", [201] = "DUZENTOS E UM", [202] = "DUZENTOS E DOIS", [203] = "DUZENTOS E TRÊS", [204] = "DUZENTOS E QUATRO",
    [205] = "DUZENTOS E CINCO", [206] = "DUZENTOS E SEIS", [207] = "DUZENTOS E SETE", [208] = "DUZENTOS E OITO", [209] = "DUZENTOS E NOVE",
    [210] = "DUZENTOS E DEZ", [211] = "DUZENTOS E ONZE", [212] = "DUZENTOS E DOZE", [213] = "DUZENTOS E TREZE", [214] = "DUZENTOS E QUATORZE",
    [215] = "DUZENTOS E QUINZE", [216] = "DUZENTOS E DEZESSEIS", [217] = "DUZENTOS E DEZESSETE", [218] = "DUZENTOS E DEZOITO", [219] = "DUZENTOS E DEZENOVE",
    [220] = "DUZENTOS E VINTE", [221] = "DUZENTOS E VINTE E UM", [222] = "DUZENTOS E VINTE E DOIS", [223] = "DUZENTOS E VINTE E TRÊS", [224] = "DUZENTOS E VINTE E QUATRO",
    [225] = "DUZENTOS E VINTE E CINCO", [226] = "DUZENTOS E VINTE E SEIS", [227] = "DUZENTOS E VINTE E SETE", [228] = "DUZENTOS E VINTE E OITO", [229] = "DUZENTOS E VINTE E NOVE",
    [230] = "DUZENTOS E TRINTA", [231] = "DUZENTOS E TRINTA E UM", [232] = "DUZENTOS E TRINTA E DOIS", [233] = "DUZENTOS E TRINTA E TRÊS", [234] = "DUZENTOS E TRINTA E QUATRO",
    [235] = "DUZENTOS E TRINTA E CINCO", [236] = "DUZENTOS E TRINTA E SEIS", [237] = "DUZENTOS E TRINTA E SETE", [238] = "DUZENTOS E TRINTA E OITO", [239] = "DUZENTOS E TRINTA E NOVE",
    [240] = "DUZENTOS E QUARENTA", [241] = "DUZENTOS E QUARENTA E UM", [242] = "DUZENTOS E QUARENTA E DOIS", [243] = "DUZENTOS E QUARENTA E TRÊS", [244] = "DUZENTOS E QUARENTA E QUATRO",
    [245] = "DUZENTOS E QUARENTA E CINCO", [246] = "DUZENTOS E QUARENTA E SEIS", [247] = "DUZENTOS E QUARENTA E SETE", [248] = "DUZENTOS E QUARENTA E OITO", [249] = "DUZENTOS E QUARENTA E NOVE",
    [250] = "DUZENTOS E CINQUENTA", [251] = "DUZENTOS E CINQUENTA E UM", [252] = "DUZENTOS E CINQUENTA E DOIS", [253] = "DUZENTOS E CINQUENTA E TRÊS", [254] = "DUZENTOS E CINQUENTA E QUATRO",
    [255] = "DUZENTOS E CINQUENTA E CINCO", [256] = "DUZENTOS E CINQUENTA E SEIS", [257] = "DUZENTOS E CINQUENTA E SETE", [258] = "DUZENTOS E CINQUENTA E OITO", [259] = "DUZENTOS E CINQUENTA E NOVE",
    [260] = "DUZENTOS E SESSENTA", [261] = "DUZENTOS E SESSENTA E UM", [262] = "DUZENTOS E SESSENTA E DOIS", [263] = "DUZENTOS E SESSENTA E TRÊS", [264] = "DUZENTOS E SESSENTA E QUATRO",
    [265] = "DUZENTOS E SESSENTA E CINCO", [266] = "DUZENTOS E SESSENTA E SEIS", [267] = "DUZENTOS E SESSENTA E SETE", [268] = "DUZENTOS E SESSENTA E OITO", [269] = "DUZENTOS E SESSENTA E NOVE",
    [270] = "DUZENTOS E SETENTA", [271] = "DUZENTOS E SETENTA E UM", [272] = "DUZENTOS E SETENTA E DOIS", [273] = "DUZENTOS E SETENTA E TRÊS", [274] = "DUZENTOS E SETENTa E QUATRO",
    [275] = "DUZENTOS E SETENTA E CINCO", [276] = "DUZENTOS E SETENTA E SEIS", [277] = "DUZENTOS E SETENTA E SETE", [278] = "DUZENTOS E SETENTA E OITO", [279] = "DUZENTOS E SETENTA E NOVE",
    [280] = "DUZENTOS E OITENTA", [281] = "DUZENTOS E OITENTA E UM", [282] = "DUZENTOS E OITENTA E DOIS", [283] = "DUZENTOS E OITENTA E TRÊS", [284] = "DUZENTOS E OITENTA E QUATRO",
    [285] = "DUZENTOS E OITENTA E CINCO", [286] = "DUZENTOS E OITENTA E SEIS", [287] = "DUZENTOS E OITENTA E SETE", [288] = "DUZENTOS E OITENTA E OITO", [289] = "DUZENTOS E OITENTA E NOVE",
    [290] = "DUZENTOS E NOVENTA", [291] = "DUZENTOS E NOVENTA E UM", [292] = "DUZENTOS E NOVENTA E DOIS", [293] = "DUZENTOS E NOVENTA E TRÊS", [294] = "DUZENTOS E NOVENTA E QUATRO",
    [295] = "DUZENTOS E NOVENTA E CINCO", [296] = "DUZENTOS E NOVENTA E SEIS", [297] = "DUZENTOS E NOVENTA E SETE", [298] = "DUZENTOS E NOVENTA E OITO", [299] = "DUZENTOS E NOVENTA E NOVE",
    [300] = "TREZENTOS", [301] = "TREZENTOS E UM", [302] = "TREZENTOS E DOIS", [303] = "TREZENTOS E TRÊS", [304] = "TREZENTOS E QUATRO",
    [305] = "TREZENTOS E CINCO", [306] = "TREZENTOS E SEIS", [307] = "TREZENTOS E SETE", [308] = "TREZENTOS E OITO", [309] = "TREZENTOS E NOVE",
    [310] = "TREZENTOS E DEZ", [311] = "TREZENTOS E ONZE", [312] = "TREZENTOS E DOZE", [313] = "TREZENTOS E TREZE", [314] = "TREZENTOS E QUATORZE",
    [315] = "TREZENTOS E QUINZE", [316] = "TREZENTOS E DEZESSEIS", [317] = "TREZENTOS E DEZESSETE", [318] = "TREZENTOS E DEZOITO", [319] = "TREZENTOS E DEZENOVE",
    [320] = "TREZENTOS E VINTE", [321] = "TREZENTOS E VINTE E UM", [322] = "TREZENTOS E VINTE E DOIS", [323] = "TREZENTOS E VINTE E TRÊS", [324] = "TREZENTOS E VINTE E QUATRO",
    [325] = "TREZENTOS E VINTE E CINCO", [326] = "TREZENTOS E VINTE E SEIS", [327] = "TREZENTOS E VINTE E SETE", [328] = "TREZENTOS E VINTE E OITO", [329] = "TREZENTOS E VINTE E NOVE",
    [330] = "TREZENTOS E TRINTA", [331] = "TREZENTOS E TRINTA E UM", [332] = "TREZENTOS E TRINTA E DOIS", [333] = "TREZENTOS E TRINTA E TRÊS", [334] = "TREZENTOS E TRINTA E QUATRO",
    [335] = "TREZENTOS E TRINTA E CINCO", [336] = "TREZENTOS E TRINTA E SEIS", [337] = "TREZENTOS E TRINTA E SETE", [338] = "TREZENTOS E TRINTA E OITO", [339] = "TREZENTOS E TRINTA E NOVE",
    [340] = "TREZENTOS E QUARENTA", [341] = "TREZENTOS E QUARENTA E UM", [342] = "TREZENTOS E QUARENTA E DOIS", [343] = "TREZENTOS E QUARENTA E TRÊS", [344] = "TREZENTOS E QUARENTA E QUATRO",
    [345] = "TREZENTOS E QUARENTA E CINCO", [346] = "TREZENTOS E QUARENTA E SEIS", [347] = "TREZENTOS E QUARENTA E SETE", [348] = "TREZENTOS E QUARENTA E OITO", [349] = "TREZENTOS E QUARENTA E NOVE",
    [350] = "TREZENTOS E CINQUENTA", [351] = "TREZENTOS E CINQUENTA E UM", [352] = "TREZENTOS E CINQUENTa E DOIS", [353] = "TREZENTOS E CINQUENTA E TRÊS", [354] = "TREZENTOS E CINQUENTA E QUATRO",
    [355] = "TREZENTOS E CINQUENTA E CINCO", [356] = "TREZENTOS E CINQUENTA E SEIS", [357] = "TREZENTOS E CINQUENTA E SETE", [358] = "TREZENTOS E CINQUENTA E OITO", [359] = "TREZENTOS E CINQUENTA E NOVE",
    [360] = "TREZENTOS E SESSENTA", [361] = "TREZENTOS E SESSENTA E UM", [362] = "TREZENTOS E SESSENTA E DOIS", [363] = "TREZENTOS E SESSENTA E TRÊS", [364] = "TREZENTOS E SESSENTA E QUATRO",
    [365] = "TREZENTOS E SESSENTA E CINCO", [366] = "TREZENTOS E SESSENTA E SEIS", [367] = "TREZENTOS E SESSENTA E SETE", [368] = "TREZENTOS E SESSENTA E OITO", [369] = "TREZENTOS E SESSENTA E NOVE",
    [370] = "TREZENTOS E SETENTA", [371] = "TREZENTOS E SETENTA E UM", [372] = "TREZENTOS E SETENTA E DOIS", [373] = "TREZENTOS E SETENTA E TRÊS", [374] = "TREZENTOS E SETENTA E QUATRO",
    [375] = "TREZENTOS E SETENTA E CINCO", [376] = "TREZENTOS E SETENTA E SEIS", [377] = "TREZENTOS E SETENTA E SETE", [378] = "TREZENTOS E SETENTA E OITO", [379] = "TREZENTOS E SETENTA E NOVE",
    [380] = "TREZENTOS E OITENTA", [381] = "TREZENTOS E OITENTA E UM", [382] = "TREZENTOS E OITENTA E DOIS", [383] = "TREZENTOS E OITENTA E TRÊS", [384] = "TREZENTOS E OITENTA E QUATRO",
    [385] = "TREZENTOS E OITENTA E CINCO", [386] = "TREZENTOS E OITENTA E SEIS", [387] = "TREZENTOS E OITENTA E SETE", [388] = "TREZENTOS E OITENTA E OUTO", [389] = "TREZENTOS E OITENTA E NOVE",
    [390] = "TREZENTOS E NOVENTA", [391] = "TREZENTOS E NOVENTA E UM", [392] = "TREZENTOS E NOVENTA E DOIS", [393] = "TREZENTOS E NOVENTA E TRÊS", [394] = "TREZENTOS E NOVENTA E QUATRO",
    [395] = "TREZENTOS E NOVENTA E CINCO", [396] = "TREZENTOS E NOVENTA E SEIS", [397] = "TREZENTOS E NOVENTA E SETE", [398] = "TREZENTOS E NOVENTA E OITO", [399] = "TREZENTOS E NOVENTA E NOVE",
    [400] = "QUATROCENTOS", [401] = "QUATROCENTOS E UM", [402] = "QUATROCENTOS E DOIS", [403] = "QUATROCENTOS E TRÊS", [404] = "QUATROCENTOS E QUATRO",
    [405] = "QUATROCENTOS E CINCO", [406] = "QUATROCENTOS E SEIS", [407] = "QUATROCENTOS E SETE", [408] = "QUATROCENTOS E OITO", [409] = "QUATROCENTOS E NOVE",
    [410] = "QUATROCENTOS E DEZ", [411] = "QUATROCENTOS E ONZE", [412] = "QUATROCENTOS E DOZE", [413] = "QUATROCENTOS E TREZE", [414] = "QUATROCENTOS E QUATORZE",
    [415] = "QUATROCENTOS E QUINZE", [416] = "QUATROCENTOS E DEZESSEIS", [417] = "QUATROCENTOS E DEZESSETE", [418] = "QUATROCENTOS E DEZOITO", [419] = "QUATROCENTOS E DEZENOVE",
    [420] = "QUATROCENTOS E VINTE", [421] = "QUATROCENTOS E VINTE E UM", [422] = "QUATROCENTOS E VINTE E DOIS", [423] = "QUATROCENTOS E VINTE E TRÊS", [424] = "QUATROCENTOS E VINTE E QUATRO",
    [425] = "QUATROCENTOS E VINTE E CINCO", [426] = "QUATROCENTOS E VINTE E SEIS", [427] = "QUATROCENTOS E VINTE E SETE", [428] = "QUATROCENTOS E VINTE E OITO", [429] = "QUATROCENTOS E VINTE E NOVE",
    [430] = "QUATROCENTOS E TRINTA", [431] = "QUATROCENTOS E TRINTA E UM", [432] = "QUATROCENTOS E TRINTA E DOIS", [433] = "QUATROCENTOS E TRINTA E TRÊS", [434] = "QUATROCENTOS E TRINTA E QUATRO",
    [435] = "QUATROCENTOS E TRINTA E CINCO", [436] = "QUATROCENTOS E TRINTA E SEIS", [437] = "QUATROCENTOS E TRINTA E SETE", [438] = "QUATROCENTOS E TRINTA E OITO", [439] = "QUATROCENTOS E TRINTA E NOVE",
    [440] = "QUATROCENTOS E QUARENTA", [441] = "QUATROCENTOS E QUARENTA E UM", [442] = "QUATROCENTOS E QUARENTA E DOIS", [443] = "QUATROCENTOS E QUARENTA E TRÊS", [444] = "QUATROCENTOS E QUARENTA E QUATRO",
    [445] = "QUATROCENTOS E QUARENTA E CINCO", [446] = "QUATROCENTOS E QUARENTA E SEIS", [447] = "QUATROCENTOS E QUARENTA E SETE", [448] = "QUATROCENTOS E QUARENTA E OITO", [449] = "QUATROCENTOS E QUARENTA E NOVE",
    [450] = "QUATROCENTOS E CINQUENTA", [451] = "QUATROCENTOS E CINQUENTA E UM", [452] = "QUATROCENTOS E CINQUENTA E DOIS", [453] = "QUATROCENTOS E CINQUENTA E TRÊS", [454] = "QUATROCENTOS E CINQUENTA E QUATRO",
    [455] = "QUATROCENTOS E CINQUENTA E CINCO", [456] = "QUATROCENTOS E CINQUENTA E SEIS", [457] = "QUATROCENTOS E CINQUENTA E SETE", [458] = "QUATROCENTOS E CINQUENTA E OITO", [459] = "QUATROCENTOS E CINQUENTA E NOVE",
    [460] = "QUATROCENTOS E SESSENTA", [461] = "QUATROCENTOS E SESSENTA E UM", [462] = "QUATROCENTOS E SESSENTA E DOIS", [463] = "QUATROCENTOS E SESSENTA E TRÊS", [464] = "QUATROCENTOS E SESSENTA E QUATRO",
    [465] = "QUATROCENTOS E SESSENTA E CINCO", [466] = "QUATROCENTOS E SESSENTA E SEIS", [467] = "QUATROCENTOS E SESSENTA E SETE", [468] = "QUATROCENTOS E SESSENTA E OITO", [469] = "QUATROCENTOS E SESSENTA E NOVE",
    [470] = "QUATROCENTOS E SETENTA", [471] = "QUATROCENTOS E SETENTA E UM", [472] = "QUATROCENTOS E SETENTA E DOIS", [473] = "QUATROCENTOS E SETENTA E TRÊS", [474] = "QUATROCENTOS E SETENTA E QUATRO",
    [475] = "QUATROCENTOS E SETENTA E CINCO", [476] = "QUATROCENTOS E SETENTA E SEIS", [477] = "QUATROCENTOS E SETENTA E SETE", [478] = "QUATROCENTOS E SETENTA E OITO", [479] = "QUATROCENTOS E SETENTA E NOVE",
    [480] = "QUATROCENTOS E OITENTA", [481] = "QUATROCENTOS E OITENTA E UM", [482] = "QUATROCENTOS E OITENTA E DOIS", [483] = "QUATROCENTOS E OITENTA E TRÊS", [484] = "QUATROCENTOS E OITENTA E QUATRO",
    [485] = "QUATROCENTOS E OITENTA E CINCO", [486] = "QUATROCENTOS E OITENTA E SEIS", [487] = "QUATROCENTOS E OITENTA E SETE", [488] = "QUATROCENTOS E OITENTA E OITO", [489] = "QUATROCENTOS E OITENTA E NOVE",
    [490] = "QUATROCENTOS E NOVENTA", [491] = "QUATROCENTOS E NOVENTA E UM", [492] = "QUATROCENTOS E NOVENTA E DOIS", [493] = "QUATROCENTOS E NOVENTA E TRÊS", [494] = "QUATROCENTOS E NOVENTA E QUATRO",
    [495] = "QUATROCENTOS E NOVENTA E CINCO", [496] = "QUATROCENTOS E NOVENTA E SEIS", [497] = "QUATROCENTOS E NOVENTa E SETE", [498] = "QUATROCENTOS E NOVENTA E OITO", [499] = "QUATROCENTOS E NOVENTA E NOVE",
    [500] = "QUINHENTOS"
}

-- Função para converter número em extenso (mantida para compatibilidade)
local function getNumeroExtenso(num)
    if numerosExtenso[num] then
        return numerosExtenso[num]
    else
        return tostring(num)
    end
end

-- FUNÇÃO PRINCIPAL PARA ENVIAR NO CHAT
local function enviarNoChat(mensagem)
    -- Método 1: Chat Events (Mais comum)
    local chatEvents = ReplicatedStorage:FindFirstChild("DefaultChatSystemChatEvents")
    if chatEvents then
        local sayEvent = chatEvents:FindFirstChild("SayMessageRequest")
        if sayEvent then
            sayEvent:FireServer(mensagem, "All")
            print("✅ Enviado via ChatEvents:", mensagem)
            return true
        end
    end
    
    -- Método 2: TextChatService (Novo sistema)
    local textChatService = game:GetService("TextChatService")
    local textChannels = textChatService:FindFirstChild("TextChannels")
    if textChannels then
        local rbxtGeneral = textChannels:FindFirstChild("RBXGeneral")
        if rbxtGeneral then
            rbxtGeneral:SendAsync(mensagem)
            print("✅ Enviado via TextChatService:", mensagem)
            return true
        end
    end
    
    -- Método 3: Chat Service direto
    local chatService = game:GetService("Chat")
    if player.Character and player.Character:FindFirstChild("Head") then
        chatService:Chat(player.Character.Head, mensagem, Enum.ChatColor.White)
        print("✅ Enviado via ChatService:", mensagem)
        return true
    end
    
    print("❌ Nenhum método de chat funcionou")
    return false
end

-- Criar GUI principal
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "AutoJsGlhub"
screenGui.Parent = playerGui
screenGui.ResetOnSpawn = false

-- Frame principal
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 280, 0, 450) -- Aumentado para caber novo controle
mainFrame.Position = UDim2.new(0.02, 0, 0.3, 0)
mainFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
mainFrame.BorderSizePixel = 2
mainFrame.BorderColor3 = Color3.fromRGB(0, 255, 0)
mainFrame.Parent = screenGui

-- Título
local titleLabel = Instance.new("TextLabel")
titleLabel.Name = "TitleLabel"
titleLabel.Size = UDim2.new(1, 0, 0, 30)
titleLabel.Position = UDim2.new(0, 0, 0, 0)
titleLabel.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
titleLabel.BorderSizePixel = 0
titleLabel.Text = "AUTO J/'S - GLHUB (RÁPIDO)"
titleLabel.TextColor3 = Color3.fromRGB(0, 0, 0)
titleLabel.TextScaled = true
titleLabel.Font = Enum.Font.Code
titleLabel.Parent = mainFrame

-- Função para criar linha de configuração
local function criarLinhaConfig(nome, posY, valorInicial)
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(0, 100, 0, 30)
    label.Position = UDim2.new(0, 10, 0, posY)
    label.BackgroundTransparency = 1
    label.Text = nome .. ":"
    label.TextColor3 = Color3.fromRGB(0, 255, 0)
    label.TextScaled = true
    label.Font = Enum.Font.Code
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = mainFrame
    
    local textBox = Instance.new("TextBox")
    textBox.Size = UDim2.new(0, 80, 0, 25)
    textBox.Position = UDim2.new(0, 120, 0, posY + 2)
    textBox.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    textBox.BorderColor3 = Color3.fromRGB(0, 255, 0)
    textBox.BorderSizePixel = 1
    textBox.Text = tostring(valorInicial)
    textBox.TextColor3 = Color3.fromRGB(0, 255, 0)
    textBox.TextScaled = true
    textBox.Font = Enum.Font.Code
    textBox.Parent = mainFrame
    
    return textBox
end

-- Criar campos de configuração
local comecarDeBox = criarLinhaConfig("Começar de", 50, 0)
local fazerAteBox = criarLinhaConfig("Fazer até", 90, 500)  -- Alterado para 500
local finalPrefixBox = criarLinhaConfig("Final Prefix", 130, 1)
local velocidadeBox = criarLinhaConfig("Velocidade", 170, 0.5) -- NOVO: Controle de velocidade

-- Campo Pular (Sim/Não)
local pularLabel = Instance.new("TextLabel")
pularLabel.Size = UDim2.new(0, 60, 0, 30)
pularLabel.Position = UDim2.new(0, 10, 0, 210)
pularLabel.BackgroundTransparency = 1
pularLabel.Text = "Pular:"
pularLabel.TextColor3 = Color3.fromRGB(0, 255, 0)
pularLabel.TextScaled = true
pularLabel.Font = Enum.Font.Code
pularLabel.TextXAlignment = Enum.TextXAlignment.Left
pularLabel.Parent = mainFrame

-- Botões Sim/Não para Pular
local simButton = Instance.new("TextButton")
simButton.Size = UDim2.new(0, 35, 0, 25)
simButton.Position = UDim2.new(0, 120, 0, 212)
simButton.BackgroundColor3 = Color3.fromRGB(0, 100, 0)
simButton.BorderColor3 = Color3.fromRGB(0, 255, 0)
simButton.BorderSizePixel = 1
simButton.Text = "sim"
simButton.TextColor3 = Color3.fromRGB(255, 255, 255)
simButton.TextScaled = true
simButton.Font = Enum.Font.Code
simButton.Parent = mainFrame

local naoButton = Instance.new("TextButton")
naoButton.Size = UDim2.new(0, 35, 0, 25)
naoButton.Position = UDim2.new(0, 165, 0, 212)
naoButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
naoButton.BorderColor3 = Color3.fromRGB(0, 255, 0)
naoButton.BorderSizePixel = 1
naoButton.Text = "não"
naoButton.TextColor3 = Color3.fromRGB(255, 255, 255)
naoButton.TextScaled = true
naoButton.Font = Enum.Font.Code
naoButton.Parent = mainFrame

-- Linha decorativa
local linha = Instance.new("TextLabel")
linha.Size = UDim2.new(1, -20, 0, 20)
linha.Position = UDim2.new(0, 10, 0, 250)
linha.BackgroundTransparency = 1
linha.Text = "----------//----------//----------//----------"
linha.TextColor3 = Color3.fromRGB(0, 255, 0)
linha.TextScaled = true
linha.Font = Enum.Font.Code
linha.Parent = mainFrame

-- Botão Iniciar
local iniciarButton = Instance.new("TextButton")
iniciarButton.Size = UDim2.new(0, 200, 0, 40)
iniciarButton.Position = UDim2.new(0, 40, 0, 290)
iniciarButton.BackgroundColor3 = Color3.fromRGB(0, 80, 0)
iniciarButton.BorderColor3 = Color3.fromRGB(0, 255, 0)
iniciarButton.BorderSizePixel = 2
iniciarButton.Text = "Iniciar"
iniciarButton.TextColor3 = Color3.fromRGB(255, 255, 255)
iniciarButton.TextScaled = true
iniciarButton.Font = Enum.Font.Code
iniciarButton.Parent = mainFrame

-- Botão Parar
local pararButton = Instance.new("TextButton")
pararButton.Size = UDim2.new(0, 200, 0, 40)
pararButton.Position = UDim2.new(0, 40, 0, 340)
pararButton.BackgroundColor3 = Color3.fromRGB(80, 0, 0)
pararButton.BorderColor3 = Color3.fromRGB(0, 255, 0)
pararButton.BorderSizePixel = 2
pararButton.Text = "parar"
pararButton.TextColor3 = Color3.fromRGB(255, 255, 255)
pararButton.TextScaled = true
pararButton.Font = Enum.Font.Code
pararButton.Parent = mainFrame

-- Status atual
local statusLabel = Instance.new("TextLabel")
statusLabel.Size = UDim2.new(1, -20, 0, 30)
statusLabel.Position = UDim2.new(0, 10, 0, 390)
statusLabel.BackgroundTransparency = 1
statusLabel.Text = "Status: Parado"
statusLabel.TextColor3 = Color3.fromRGB(0, 255, 0)
statusLabel.TextScaled = true
statusLabel.Font = Enum.Font.Code
statusLabel.Parent = mainFrame

-- Função para atualizar configurações
local function atualizarConfigs()
    config.comecarDe = tonumber(comecarDeBox.Text) or 0
    config.fazerAte = tonumber(fazerAteBox.Text) or 500  -- Alterado para 500
    config.finalPrefix = tonumber(finalPrefixBox.Text) or 1
    config.velocidade = tonumber(velocidadeBox.Text) or 0.5 -- NOVO: Atualizar velocidade
end

-- Função para alternar pular
simButton.MouseButton1Click:Connect(function()
    config.pular = true
    simButton.BackgroundColor3 = Color3.fromRGB(0, 100, 0)
    naoButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
end)

naoButton.MouseButton1Click:Connect(function()
    config.pular = false
    naoButton.BackgroundColor3 = Color3.fromRGB(100, 0, 0)
    simButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
end)

-- Função principal do loop (CORRIGIDA E ACELERADA)
local function executarLoop()
    atualizarConfigs()
    currentNumber = config.comecarDe
    isRunning = true
    
    statusLabel.Text = "Status: Executando..."
    print("🚀 Iniciando AUTO J/'S - GLHUB (RÁPIDO)")
    print("📊 Configurações:")
    print("   - Começar de:", config.comecarDe)
    print("   - Fazer até:", config.fazerAte)
    print("   - Final Prefix:", config.finalPrefix)
    print("   - Pular:", config.pular and "Sim" or "Não")
    print("   - Velocidade:", config.velocidade, "segundos") -- NOVO: Log de velocidade
    
    spawn(function()
        while isRunning and currentNumber <= config.fazerAte do
            -- Verificar se deve pular este número
            if config.pular and currentNumber % 2 == 0 then
                print("⏭️ Pulando número:", currentNumber)
                statusLabel.Text = "Status: Pulando " .. getNumeroExtenso(currentNumber)
            else
                local numeroTexto = getNumeroExtenso(currentNumber)
                local mensagem = numeroTexto .. " !"
                
                -- Adicionar prefix final se configurado
                if config.finalPrefix > 1 then
                    for i = 2, config.finalPrefix do
                        mensagem = mensagem .. " !"
                    end
                end
                
                -- ENVIAR NO CHAT
                enviarNoChat(mensagem)
                statusLabel.Text = "Status: Enviando " .. numeroTexto
            end
            
            -- Incrementar número (SEMPRE incrementa 1, independente da opção pular)
            currentNumber = currentNumber + 1
            
            wait(config.velocidade) -- ALTERADO: Usar velocidade configurada em vez de 2 segundos fixos
        end
        
        if isRunning then
            statusLabel.Text = "Status: Concluído! ✅"
            print("✅ AUTO J/'S concluído!")
        end
        isRunning = false
    end)
end

-- Função para parar
local function pararLoop()
    isRunning = false
    statusLabel.Text = "Status: Parado"
    print("⏹️ AUTO J/'S parado pelo usuário")
end

-- Conectar botões
iniciarButton.MouseButton1Click:Connect(function()
    if not isRunning then
        executarLoop()
    end
end)

pararButton.MouseButton1Click:Connect(function()
    pararLoop()
end)

-- Tornar a GUI arrastável
local dragging = false
local dragStart = nil
local startPos = nil

local function updateInput(input)
    local delta = input.Position - dragStart
    mainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
end

titleLabel.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = mainFrame.Position
        
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

titleLabel.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement and dragging then
        updateInput(input)
    end
end)

print("🎯 AUTO J/'S - GLHUB (RÁPIDO) carregado!")
print("📝 Abra o console (F9) para ver os logs")
print("💬 Configure os valores e clique 'Iniciar'")
print("⚡ Use valores menores em 'Velocidade' para escrever mais rápido")
