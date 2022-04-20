# leo
(função(){
    const __exports = {};
    deixe-me;

    const heap = new Array(32);

    heap.fill(indefinido);

    heap.push(undefined, null, true, false);

    deixe stack_pointer = 32;

    function addBorowedObject(obj) {
        if (stack_pointer == 1) throw new Error('out of js stack');
        heap[--stack_pointer] = obj;
        return stack_pointer;
    }

function getObject(idx) { return heap[idx]; }

deixe heap_next = heap.length;

função dropObject(idx) {
    se (idx < 36) retornar;
    heap[idx] = heap_next;
    heap_next = idx;
}

function takeObject(idx) {
    const ret = getObject(idx);
    dropObject(idx);
    retorno ret;
}

let cachedTextDecoder = new TextDecoder('utf-8');

deixe cachegetUint8Memory = null;
function getUint8Memory() {
    if (cachegetUint8Memory === null || cachegetUint8Memory.buffer !== wasm.memory.buffer) {
        cachegetUint8Memory = new Uint8Array(wasm.memory.buffer);
    }
    return cachegetUint8Memory;
}

function getStringFromWasm(ptr, len) {
    return cachedTextDecoder.decode(getUint8Memory().subarray(ptr, ptr + len));
}

function addHeapObject(obj) {
    if (heap_next === heap.length) heap.push(heap.length + 1);
    const idx = heap_next;
    heap_next = heap[idx];

    heap[idx] = obj;
    retornar idx;
}

função handleError(e) {
    wasm.__wbindgen_exn_store(addHeapObject(e));
}

deixe WASM_VECTOR_LEN = 0;

let cachedTextEncoder = new TextEncoder('utf-8');

deixe passStringToWasm;
if (typeof cachedTextEncoder.encodeInto === 'function') {
    passStringToWasm = function(arg) {


        deixe tamanho = arg.length;
        deixe ptr =




 wasm.__wbindgen_malloc(tamanho);
        deixe deslocamento = 0;
        {
            const mem = getUint8Memory();
            for (; deslocamento < comprimento do arg; deslocamento++) {
                código const = arg.charCodeAt(offset);
                se (código > 0x7F) quebrar;
                mem[ptr + deslocamento] = código;
            }
        }

        if (offset !== arg.length) {
            arg = arg.fatia(deslocamento);
            ptr = wasm.__wbindgen_realloc(ptr, tamanho, tamanho = deslocamento + arg.length * 3);
            const view = getUint8Memory().subarray(ptr + deslocamento, ptr + tamanho);
            const ret = cachedTextEncoder.encodeInto(arg, view);

            offset += ret.escrito;
        }
        WASM_VECTOR_LEN = deslocamento;
        retornar pt;
    };
} senão {
    passStringToWasm = function(arg) {


        deixe tamanho = arg.length;
        let ptr = wasm.__wbindgen_malloc(tamanho);
        deixe deslocamento = 0;
        {
            const mem = getUint8Memory();
            for (; deslocamento < comprimento do arg; deslocamento++) {
                código const = arg.charCodeAt(offset);
                se (código > 0x7F) quebrar;
                mem[ptr + deslocamento] = código;
            }
        }

        if (offset !== arg.length) {
            const buf = cachedTextEncoder.encode(arg.slice(offset));
            ptr = wasm.__wbindgen_realloc(ptr, tamanho, tamanho = deslocamento + buf.comprimento);
            getUint8Memory().set(buf, ptr + offset);
            deslocamento += buf.comprimento;
        }
        WASM_VECTOR_LEN = deslocamento;
        retornar pt;
    };
}

deixe cachegetUint32Memory = null;
function getUint32Memory() {
    if (cachegetUint32Memory === null || cachegetUint32Memory.buffer !== wasm.memory.buffer) {
        cachegetUint32Memory = new Uint32Array(wasm.memory.buffer);
    }
    return cachegetUint32Memory;
}

deixe cachegetInt32Memory = null;
function getInt32Memory() {
    if (cachegetInt32Memory === null || cachegetInt32Memory.buffer !== wasm.memory.buffer) {
        cachegetInt32Memory = new Int32Array(wasm.memory.buffer);
    }
    return cachegetInt32Memory;
}

function debugString(val) {
    // tipos primitivos
    const tipo = tipo de val;
    if (type == 'number' || type == 'boolean' || val == null) {
        return `${val}`;
    }
    if (tipo == 'string') {
        return `"${val}"`;
    }
    if (tipo == 'símbolo') {
        const descrição = val.descrição;
        if (descrição == null) {
            return 'Símbolo';
        } senão {
            return `Símbolo(${descrição})`;
        }
    }
    if (tipo == 'função') {
        const nome = val.nome;
        if (typeof name == 'string' && name.length > 0) {
            return `Função(${nome})`;
        } senão {
            return 'Função';
        }
    }
    // objetos
    if (Array.isArray(val)) {
        const comprimento = val.comprimento;
        deixe depurar = '[';
        if (comprimento > 0) {
            depurar += debugString(val[0]);
        }
        for(seja i = 1; i < comprimento; i++) {
            debug += ', ' + debugString(val[i]);
        }
        depurar += ']';
        retornar depuração;
    }
    //Teste para built-in
    const builtInMatches = /\[object ([^\]]+)\]/.exec(toString.call(val));
    deixe nomedaclasse;
    if (builtInMatches.length > 1) {
        className = builtInMatches[1];
    } senão {
        // Falha ao corresponder ao padrão '[object ClassName]'
        return toString.call(val);
    }
    if (className == 'Objeto') {
        // somos uma classe ou objeto definido pelo usuário
        // JSON.stringify evita problemas com ciclos e geralmente é muito
        // mais fácil do que fazer um loop através de ownProperties de `val`.
        experimentar {
            return 'Objeto(' + JSON.stringify(val) + ')';
        } pegar (_) {
            return 'Objeto';
        }
    }
    // erros
    if (val instanceof Error) {
        return `${val.name}: ${val.message}\n${val.stack}`;
    }
    // TODO poderíamos testar mais coisas aqui, como `Set`s e `Map`s.
    return nomedaclasse;
}
/**
* Um novo tipo para conjuntos de regras, envolvendo toda a funcionalidade JS
*/
classe Conjuntos de regras {

    estático __wrap(ptr) {
        const obj = Object.create(RuleSets.prototype);
        obj.ptr = ptr;

        retornar obj;
    }

    gratuitamente() {
        const ptr = this.ptr;
        this.ptr = 0;

        wasm.__wbg_rulesets_free(ptr);
    }
    /**
    * Retorna uma nova estrutura JsRulesets
    * @returns {RuleSets}
    */
    estático novo() {
        const ret = wasm.rulesets_new();
        return RuleSets.__wrap(ret);
    }
    /**
    * Retorna o número de alvos na estrutura JsRuleSets atual como um `usize`
    * @returns {número}
    */
    count_targets() {
        const ret = wasm.rulesets_count_targets(this.ptr);
        return ret >>> 0;
    }
    /**
    * Construa e adicione novos conjuntos de regras com base em uma matriz de valores JS
    *
    *#Argumentos
    *
    * * `array` - Um objeto JS Array de conjuntos de regras
    * * `enable_mixed_rulesets` - Um JS Boolean indicando se conjuntos de regras que acionam mistos
    * o bloqueio de conteúdo deve estar ativado
    * * `rule_active_states` - Um objeto JS que nos permite saber se os conjuntos de regras foram desabilitados
    * ou habilitado
    * * `scope` - Uma string JS opcional que indica o escopo do lote atual de
    * conjuntos de regras sendo adicionados (consulte a documentação dos [canais de atualização do conjunto de regras](https://github.com/EFForg/https-everywhere/blob/master/docs/en_US/ruleset-update-channels.md))
    * @param {qualquer} array
    * @param {qualquer} enable_mixed_rulesets
    * @param {qualquer} rule_active_states
    * @param {qualquer} escopo
    */
    add_all_from_js_array(array, enable_mixed_rulesets, rule_active_states, escopo) {
        experimentar {
            wasm.rulesets_add_all_from_js_array(this.ptr, addBorrowedObject(array), addBorrowedObject(enable_mixed_rulesets), addBorrowedObject(rule_active_states), addBorrowedObject(escopo));
        } finalmente {
            heap[stack_pointer++] = indefinido;
            heap[stack_pointer++] = indefinido;
            heap[stack_pointer++] = indefinido;
            heap[stack_pointer++] = indefinido;
        }
    }
    /**
    * Remova um RuleSet da estrutura RuleSets
    * @param {qualquer} conjunto de regras_jsval
    */
    remove_ruleset(ruleset_jsval) {
        experimentar {
            wasm.rulesets_remove_ruleset(this.ptr, addBorowedObject(ruleset_jsval));
        } finalmente {
            heap[stack_pointer++] = indefinido;
        }
    }
    /**
    * Retorna um array JS de conjuntos de regras que estão ativos e não têm exclusões, para todos os hosts que
    * estão em um único conjunto de regras e terminam no final fornecido
    *
    *#Argumentos
    *
    * * `ending` - Uma string JS que indica o destino final a ser pesquisado
    * @param {qualquer} terminando
    * @returns {qualquer}
    */
    get_simple_rules_ending_with(terminando) {
        experimentar {
            const ret = wasm.rulesets_get_simple_rules_ending_with(this.ptr, addBorowedObject(final));
            return takeObject(ret);
        } finalmente {
            heap[stack_pointer++] = indefinido;
        }
    }
    /**
    * Retorna um conjunto JS de conjuntos de regras que se aplicam ao host fornecido
    *
    *#Argumentos
    *
    * * `host` - Uma string JS que indica o host para procurar conjuntos de regras potencialmente aplicáveis
    * @param {qualquer} host
    * @returns {qualquer}
    */
    potencialmente_aplicável(host) {
        experimentar {
            const ret = wasm.rulesets_potentially_applicable(this.ptr, addBorowedObject(host));
            return takeObject(ret);
        } finalmente {
            heap[stack_pointer++] = indefinido;
        }
    }
}
__exports.RuleSets = Conjuntos de Regras;

função init(módulo) {

    deixe resultado;
    const importa = {};
    imports.wbg = {};
    imports.wbg.__wbindgen_object_drop_ref = function(arg0) {
        takeObject(arg0);
    };
    imports.wbg.__wbindgen_string_new = function(arg0, arg1) {
        const ret = getStringFromWasm(arg0, arg1);
        return addHeapObject(ret);
    };
    imports.wbg.__wbindgen_is_object = function(arg0) {
        const val = getObject(arg0);
        const ret = typeof(val) === 'objeto' && val !== null;
        retorno ret;
    };
    imports.wbg.__wbindgen_jsval_eq = function(arg0, arg1) {
        const ret = getObject(arg0) === getObject(arg1);
        retorno ret;
    };
    imports.wbg.__wbindgen_is_undefined = function(arg0) {
        const ret = getObject(arg0) === indefinido;
        retorno ret;
    };
    imports.wbg.__wbindgen_is_null = function(arg0) {
        const ret = getObject(arg0) === null;
        retorno ret;
    };
    imports.wbg.__wbindgen_object_clone_ref = function(arg0) {
        const ret = getObject(arg0);
        return addHeapObject(ret);
    };
    imports.wbg.__wbg_valueOf_ba9b8eea6574e4cd = function(arg0) {
        const ret = getObject(arg0).valueOf();
        retorno ret;
    };
    imports.wbg.__wbg_done_d485bad1edfcebc6 = function(arg0) {
        const ret = getObject(arg0).done;
        retorno ret;
    };
    imports.wbg.__wbg_value_ce1d7ad603d82534 = function(arg0) {
        const ret = getObject(arg0).valor;
        return addHeapObject(ret);
    };
    imports.wbg.__wbg_new_f1f0f3113e466334 = function() {
        const ret = new Array();
        return addHeapObject(ret);
    };
    imports.wbg.__wbg_from_de9ef68c3ad78100 = function(arg0) {
        const ret = Array.from(getObject(arg0));
        return addHeapObject(ret);
    };
    imports.wbg.__wbg_isArray_a354dddde485c5cb = function(arg0) {
        const ret = Array.isArray(getObject(arg0));
        retorno ret;
    };
    imports.wbg.__wbg_length_1fb83c219763252e = function(arg0) {
        const ret = getObject(arg0).length;
        retorno ret;
    };
    imports.wbg.__wbg_push_829cf1fbae322d44 = function(arg0, arg1) {
        const ret = getObject(arg0).push(getObject(arg1));
        retorno ret;
    };
    imports.wbg.__wbg_values_65bd19cf6fdcc3f3 = function(arg0) {
        const ret = getObject(arg0).values();
        return addHeapObject(ret);
    };
    imports.wbg.__wbg_next_17e79f725ff5a87a = function(arg0) {
        experimentar {
            const ret = getObject(arg0).next();
            return addHeapObject(ret);
        } pegar (e) {
            handleError(e)
        }
    };
    imports.wbg.__wbg_new_abadb45e63451a4b = function() {
        const ret = new Objeto();
        return addHeapObject(ret);
    };
    imports.wbg.__wbg_new_8b2f143a50ad38e8 = function(arg0, arg1, arg2, arg3) {
        const ret = new RegExp(getStringFromWasm(arg0, arg1), getStringFromWasm(arg2, arg3));
        return addHeapObject(ret);
    };
    imports.wbg.__wbg_test_1c1b53250bceac51 = function(arg0, arg1, arg2) {
        const ret = getObject(arg0).test(getStringFromWasm(arg1, arg2));
        retorno ret;
    };
    imports.wbg.__wbg_toString_bc87920f1a2b55ac = function(arg0) {
        const ret = getObject(arg0).toString();
        return addHeapObject(ret);
    };
    imports.wbg.__wbg_add_5aa1e13fed2f3154 = function(arg0, arg1) {
        const ret = getObject(arg0).add(getObject(arg1));
        return addHeapObject(ret);
    };
    imports.wbg.__wbg_new_1f23e55940712795 = function(arg0) {
        const ret = new Set(getObject(arg0));
        return addHeapObject(ret);
    };
    imports.wbg.__wbg_get_f54fa02552389dda = function(arg0, arg1) {
        experimentar {
            const ret = Reflect.get(getObject(arg0), getObject(arg1));
            return addHeapObject(ret);
        } pegar (e) {
            handleError(e)
        }
    };
    imports.wbg.__wbg_set_09ce0f7f67d68b09 = function(arg0, arg1, arg2) {
        experimentar {
            const ret = Reflect.set(getObject(arg0), getObject(arg1), getObject(arg2));
            retorno ret;
        } pegar (e) {
            handleError(e)
        }
    };
    imports.wbg.__wbindgen_is_string = function(arg0) {
        const ret = typeof(getObject(arg0)) === 'string';
        retorno ret;
    };
    imports.wbg.__wbindgen_string_get = function(arg0, arg1) {
        const obj = getObject(arg0);
        if (typeof(obj) !== 'string') return 0;
        const ptr = passStringToWasm(obj);
        getUint32Memory()[arg1 / 4] = WASM_VECTOR_LEN;
        const ret = ptr;
        retorno ret;
    };
    imports.wbg.__wbindgen_boolean_get = function(arg0) {
        const v = getObject(arg0);
        const ret = typeof(v) === 'boolean' ? (v? 1:0): 2;
        retorno ret;
    };
    imports.wbg.__wbindgen_debug_string = function(arg0, arg1) {
        const ret = debugString(getObject(arg1));
        const ret0 = passStringToWasm(ret);
        const ret1 = WASM_VECTOR_LEN;
        getInt32Memory()[arg0 / 4 + 0] = ret0;
        getInt32Memory()[arg0 / 4 + 1] = ret1;
    };
    imports.wbg.__wbindgen_throw = function(arg0, arg1) {
        throw new Error(getStringFromWasm(arg0, arg1));
    };

    if (module instanceof URL || typeof module === 'string' || module instanceof Request) {

        resposta const = buscar(módulo);
        if (typeof WebAssembly.instantiateStreaming === 'function') {
            resultado = WebAssembly.instantiateStreaming(resposta, importações)
            .catch(e => {
                console.warn("`WebAssembly.instantiateStreaming` falhou. Supondo que isso seja porque seu servidor não serve wasm com o tipo MIME `application/wasm`. Voltando para `WebAssembly.instantiate` que é mais lento. Erro original:\n", e);
                resposta de retorno
                .then(r => r.arrayBuffer())
                .then(bytes => WebAssembly.instantiate(bytes, importações));
            });
        } senão {
            resultado = resposta
            .then(r => r.arrayBuffer())
            .then(bytes => WebAssembly.instantiate(bytes, importações));
        }
    } senão {

        resultado = WebAssembly.instantiate(module, imports)
        .then(resultado => {
            if (resultado instância de WebAssembly.Instance) {
                return { instância: resultado, módulo };
            } senão {
                retorno resultado;
            }
        });
    }
    return resultado.then(({instância, módulo}) => {
        wasm = instância.exports;
        init.__wbindgen_wasm_module = módulo;

        retorno era;
    });
}

self.wasm_bindgen = Object.assign(init, __exports);

})();
