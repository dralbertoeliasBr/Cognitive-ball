#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
GAIA_v1.1.py  (02/11/2026)  -  o GaIA funcional, tudo num arquivo
v1.1: troca de senha sem perder nada (diga "trocar senha").
v1.2: toda vez que roda, faz o COFRE sozinho: copia seus codigos e a Casa
      para um zip novo (so se algo mudou), aqui e no iCloud. Assim voce pode
      limpar o iPhone sem perder nada.
v1.5 (04/11/2026): (1) NAO copia mais as respostas para a area de transferencia
      (isso estragava o "colar" do Sounavy); diga "copiar" para copiar a ultima resposta.
      (2) Com a Casa vazia, "trocar senha" nao pede a senha atual (nao ha nada a proteger).
      (3) Se colar um comando de terminal no chat, avisa em vez de tentar responder.
v1.4 (04/11/2026): ARQUIVISTA AUTOMATICO que le POR DENTRO (nunca pelo nome).
      Toda vez que o GaIA abre, ele esvazia o Inox sozinho. Inox tem duas portas:
      GAIA_CASA/00_INOX (aqui) e iCloud Drive > Pythonista 3 > GAIA_INOX (de qualquer app,
      pelo "Salvar em Arquivos"). Filtro do Guardiao (CPF, cartao, e-mail, telefone, CEP,
      senha, chave) manda para a Black e nunca deixa sair para o Claude. Atalho:
      pythonista3://GAIA.py?action=run&argv=arquivista
v1.3: fora do Pythonista (Code App, a-Shell) lembra a senha de TESTE num
      arquivo da propria Casa (GAIA_CASA/.senha_teste), so neste celular,
      e a senha nao aparece na tela ao digitar. As versoes anteriores continuam intactas.
Conceito: Sounavy (Antonio Alberto Lopes Elias, ORCID 0000-0002-5602-9916)
Codigo: Claude. Arquivo novo; nao altera nenhum anterior.

O ciclo:  voce fala -> GaIA entende -> o Delta decide -> guarda -> responde em voz
  Corpo  : a Casa (Inox + Arquivista + 7 caixas, Black 2/3)   [de GAIA_CASA v1]
  Cerebro: a escada do Delta, sobe so quando o degrau de baixo nao resolve
           N0 Reflexo  conta, hora, data                         custo 1
           N1 Comando  guarda / busca / lista / pega o Inox      custo 3
           N2 Memoria  responde com o que ja esta nas caixas     custo 5
           N3 Claude   so se a confianca ficar abaixo de 0,80    custo 11
  Regra : LEMBRADO (ja estava na Casa) e diferente de APRENDIDO (veio do Claude).
          O que o Claude ensina vai para a 01 Verde marcado APRENDIDO.
          A Black (07) nunca vai para o Claude nem aparece em busca.
  Voz   : no Pythonista fala a resposta (speech.say) e copia para a area de
          transferencia. Para falar com o GaIA por voz: Atalho do iPhone
          "Ditar texto" -> "Run Pythonista Script" GAIA.py com o texto.

Uso: copiar, colar no Pythonista e apertar Play (conversa pelo teclado,
o microfone do teclado tambem dita). Ou pelo Atalho, com o texto ditado.
A chave do Claude, se quiser o N3: guarde voce mesmo no Keychain do
Pythonista com  keychain.set_password("GAIA", "claude", "SUA_CHAVE").
Ela nunca vai para arquivo nem para conversa.
"""

import os, json, hmac, hashlib, shutil, base64, secrets
from datetime import datetime

HOME = os.path.expanduser("~")
DOCS = os.path.join(HOME, "Documents")
CASA = os.path.join(DOCS if os.path.isdir(DOCS) else HOME, "GAIA_CASA")
_ICL = os.path.join("Library", "Mobile Documents", "iCloud~com~omz-software~Pythonista3", "Documents")
# v1.4: no iPhone o iCloud do Pythonista pode estar em caminhos diferentes (achado do Arquivista v4, 03/11)
CANDIDATOS_ICLOUD = [os.path.join(HOME, _ICL), os.path.join("/private/var/mobile", _ICL),
                     os.path.join("/var/mobile", _ICL)]
ICLOUD = next((c for c in CANDIDATOS_ICLOUD if os.path.isdir(c)), CANDIDATOS_ICLOUD[0])

CAIXAS = {
    "00": "00_INOX",
    "01": "01_VERDE", "02": "02_AZUL", "03": "03_AMARELO", "04": "04_LARANJA",
    "05": "05_RED", "06": "06_VIOLETA", "07": "07_BLACK",
}
INDICE = os.path.join(CASA, "_indice.jsonl")       # cresce, nunca reescreve
MANIFESTO = os.path.join(CASA, "_manifesto.log")
FEITO = os.path.join(CASA, CAIXAS["00"], "_feito")
NUVEM_GITHUB = os.path.join(CASA, "07_BLACK", "_para_github")


# ------------------------------------------------------------ casa
def montar_casa():
    for nome in CAIXAS.values():
        os.makedirs(os.path.join(CASA, nome), exist_ok=True)
    os.makedirs(FEITO, exist_ok=True)
    os.makedirs(NUVEM_GITHUB, exist_ok=True)


def log(evento):
    with open(MANIFESTO, "a", encoding="utf-8") as f:
        f.write("{} | {}\n".format(datetime.now().isoformat(), evento))


def ler_indice():
    itens = []
    if os.path.exists(INDICE):
        with open(INDICE, "r", encoding="utf-8") as f:
            for linha in f:
                linha = linha.strip()
                if linha:
                    try:
                        itens.append(json.loads(linha))
                    except ValueError:
                        pass
    return itens


def anotar_indice(item):
    with open(INDICE, "a", encoding="utf-8") as f:
        f.write(json.dumps(item, ensure_ascii=False) + "\n")


# ------------------------------------------------------------ chave
def obter_chave(senha=None):
    """Chave do selo. Ordem: senha passada > Keychain do Pythonista > pergunta.
    Nunca grava a chave em arquivo."""
    if senha:
        return senha.encode("utf-8")
    try:
        import keychain  # so existe no Pythonista
        k = keychain.get_password("GAIA_CASA", "selo")
        if not k:
            k = input("Crie a senha da Casa (minimo 8): ").strip()
            if len(k) < 8:
                raise SystemExit("Senha curta. Nada foi gravado.")
            keychain.set_password("GAIA_CASA", "selo", k)
            print("Senha guardada no Keychain do iPhone (nao em arquivo).")
        return k.encode("utf-8")
    except ImportError:
        arq = os.path.join(CASA, ".senha_teste")
        if os.path.exists(arq):
            with open(arq, "r", encoding="utf-8") as f:
                k = f.read().strip()
            if len(k) >= 8:
                return k.encode("utf-8")
        k = _pedir("Senha da Casa (minimo 8): ")
        if len(k) < 8:
            raise SystemExit("Senha curta. Nada foi gravado.")
        if _pedir("Lembrar neste celular (modo teste)? s/n: ", oculto=False).lower().startswith("s"):
            os.makedirs(CASA, exist_ok=True)
            with open(arq, "w", encoding="utf-8") as f:
                f.write(k)
            try:
                os.chmod(arq, 0o600)
            except OSError:
                pass
            print("Senha de teste guardada em GAIA_CASA/.senha_teste (so neste celular).")
        return k.encode("utf-8")


def _pedir(texto, oculto=True):
    if oculto:
        try:
            import getpass
            return getpass.getpass(texto).strip()
        except Exception:
            pass
    return input(texto).strip()


def selar(chave, dados):
    return hmac.new(chave, dados, hashlib.sha256).hexdigest()


# ------------------------------------------------------------ nome-informacao
def nome_info(caixa, quando, selo, extra=""):
    # 7 + caixa + AAAAMMDD-HHMMSS + 8 letras do selo  (o texto nunca entra no nome)
    return "7{}-{}-{}{}".format(caixa, quando.strftime("%Y%m%d-%H%M%S"), selo[:8],
                                "-" + extra if extra else "")


# ------------------------------------------------------------ cifra da Black
# So biblioteca padrao: fluxo SHA-256 em modo contador + HMAC (cifra autenticada).
def _fluxo(chave, nonce, n):
    saida = bytearray()
    c = 0
    while len(saida) < n:
        saida += hashlib.sha256(chave + nonce + c.to_bytes(8, "big")).digest()
        c += 1
    return bytes(saida[:n])


def _derivar(senha_bytes, sal):
    return hashlib.pbkdf2_hmac("sha256", senha_bytes, sal, 200000, dklen=64)


def trancar(chave, dados):
    sal, nonce = secrets.token_bytes(16), secrets.token_bytes(16)
    k = _derivar(chave, sal)
    ct = bytes(a ^ b for a, b in zip(dados, _fluxo(k[:32], nonce, len(dados))))
    tag = hmac.new(k[32:], sal + nonce + ct, hashlib.sha256).digest()
    return sal + nonce + tag + ct


def destrancar(chave, pacote):
    sal, nonce, tag, ct = pacote[:16], pacote[16:32], pacote[32:64], pacote[64:]
    k = _derivar(chave, sal)
    if not hmac.compare_digest(tag, hmac.new(k[32:], sal + nonce + ct, hashlib.sha256).digest()):
        raise ValueError("senha errada ou pedaco adulterado")
    return bytes(a ^ b for a, b in zip(ct, _fluxo(k[:32], nonce, len(ct))))


def _xor(a, b):
    return bytes(x ^ y for x, y in zip(a, b))


def dividir_em_3(pacote):
    m = (len(pacote) + 1) // 2
    A = pacote[:m]
    B = pacote[m:].ljust(m, b"\0")
    return [A, B, _xor(A, B)]


def juntar_2(pedacos, tamanho):
    A = pedacos.get(1) or _xor(pedacos[2], pedacos[3])
    B = pedacos.get(2) or _xor(pedacos[1], pedacos[3])
    return (A + B)[:tamanho]


# ------------------------------------------------------------ Arquivista
def arquivar(chave, dados, caixa, origem="texto", quando=None):
    """Guarda bytes na caixa. Devolve o registro do indice."""
    caixa = str(caixa).zfill(2)
    if caixa not in CAIXAS or caixa == "00":
        raise ValueError("Caixa invalida: {}".format(caixa))
    quando = quando or datetime.now()
    conteudo_sha = hashlib.sha256(dados).hexdigest()

    # Stop/Go: o mesmo conteudo na mesma caixa nao entra de novo
    for it in indice_vigente():
        if it.get("sha") == conteudo_sha and it.get("caixa") == caixa:
            log("STOP duplicado {} caixa {}".format(conteudo_sha[:12], caixa))
            return dict(it, status="STOP")

    selo = selar(chave, dados)
    registro = {"caixa": caixa, "quando": quando.isoformat(), "sha": conteudo_sha,
                "selo": selo, "origem": origem, "tamanho": len(dados)}

    if caixa == "07":
        pacote = trancar(chave, dados)
        ped = dividir_em_3(pacote)
        base = nome_info("07", quando, selo)
        destinos = {1: os.path.join(CASA, CAIXAS["07"])}
        destinos[2] = os.path.join(ICLOUD, "GAIA_CASA_BLACK") if os.path.isdir(ICLOUD) \
            else os.path.join(CASA, CAIXAS["07"], "_nuvem_icloud")
        destinos[3] = NUVEM_GITHUB
        caminhos = {}
        for i in (1, 2, 3):
            os.makedirs(destinos[i], exist_ok=True)
            c = os.path.join(destinos[i], "{}-p{}.bin".format(base, i))
            with open(c, "wb") as f:
                f.write(ped[i - 1])
            caminhos[i] = c
        registro.update({"nome": base, "pedacos": caminhos, "tamanho_pacote": len(pacote),
                         "selo_pacote": hashlib.sha256(pacote).hexdigest()})
    else:
        extensao = ".txt" if origem in ("texto", "voz", "claude") else os.path.splitext(origem)[1]
        nome = nome_info(caixa, quando, selo) + (extensao or ".bin")
        caminho = os.path.join(CASA, CAIXAS[caixa], nome)
        with open(caminho, "wb") as f:
            f.write(dados)
        registro.update({"nome": nome, "caminho": caminho})

    anotar_indice(registro)
    log("GO {} caixa {} origem {}".format(registro["nome"], caixa, origem))
    registro["status"] = "GO"
    return registro


def caixa_pelo_nome(nome_arquivo, padrao="03"):
    """O nome ja e informacao: '05_qualquer.txt' ou 'red_...' vai para a 05."""
    n = nome_arquivo.lower()
    if n[:2] in CAIXAS and n[2:3] in ("_", "-", " ") and n[:2] != "00":
        return n[:2]
    palavras = {"verde": "01", "azul": "02", "amarelo": "03", "laranja": "04",
                "red": "05", "vermelho": "05", "violeta": "06", "violet": "06",
                "black": "07", "preto": "07"}
    for p, c in palavras.items():
        if n.startswith(p):
            return c
    return padrao


# ============================================================ ARQUIVISTA por dentro (v1.4)
# Regra do Sounavy: nunca decidir pelo nome. O nome so serve de endereco.
PADROES_PRIVADOS = [                                   # do GaIA Guardiao v1.1 (04/11/2026)
    ("CPF", r"\b\d{3}\.?\d{3}\.?\d{3}-\d{2}\b"),
    ("CARTAO", r"\b(?:\d{4}[ -]){3}\d{4}\b"),
    ("EMAIL", r"[\w.+-]+@[\w-]+\.[\w.]+"),
    ("TELEFONE", r"\(?\b\d{2}\)?\s?9\d{4}-\d{4}\b"),
    ("CEP", r"\b\d{5}-\d{3}\b"),
]
PALAVRAS_PRIVADAS = ("senha", "password", "api_key", "apikey", "token", "segredo", "prontuario",
                     "laudo", "paciente", "contrato", "rg ", "conta bancaria", "pix", "cofre", "privado")
PALAVRAS_PROJETO = ("projeto", "gaia", "enclave", "livro", "tese", "capitulo", "clinica", "ideia",
                    "arquitetura", "manifesto", "zenodo")
MIDIA = ((b"\xff\xd8\xff", "foto JPG"), (b"\x89PNG", "imagem PNG"), (b"GIF8", "imagem GIF"),
         (b"ID3", "audio MP3"), (b"%PDF", "PDF"))


def privados_no_texto(texto):
    achou = [n for n, p in PADROES_PRIVADOS if re.search(p, texto)]
    t = sem_acento(texto)
    achou += ["palavra '" + w.strip() + "'" for w in PALAVRAS_PRIVADAS if w in t]
    if re.search(r"sk-[A-Za-z0-9_-]{16,}", texto):
        achou.append("chave de API")
    return achou


def classificar_por_dentro(dados):
    """Le o conteudo e devolve (caixa, motivo). Nunca olha o nome do arquivo."""
    cabeca = dados[:16]
    for assinatura, tipo in MIDIA:
        if cabeca.startswith(assinatura):
            return ("03", tipo + " (trabalho)") if tipo == "PDF" else ("02", tipo)
    if len(cabeca) >= 12 and cabeca[4:8] == b"ftyp":
        return "02", "video/audio"
    try:
        texto = dados.decode("utf-8")
    except UnicodeDecodeError:
        return "03", "arquivo binario (sem texto para ler)"
    if re.search(r"(?m)^\s*(def |class |import |from \w+ import |function |const |#!|<!doctype|<html)", texto, re.I):
        # codigo fala de "senha" e "cofre" o tempo todo: so vai para a Black se tiver um segredo DE VERDADE
        reais = [n for n, pp in PADROES_PRIVADOS if re.search(pp, texto)]
        if re.search(r"sk-[A-Za-z0-9_-]{16,}", texto):
            reais.append("chave de API")
        return ("07", "codigo com segredo: " + ", ".join(reais[:3])) if reais else ("06", "codigo")
    priv = privados_no_texto(texto)
    if priv:
        return "07", "privado: " + ", ".join(priv[:3])
    t = sem_acento(texto)
    achou = [w for w in PALAVRAS_PROJETO if re.search(r"\b" + w + r"\b", t)]
    if achou:
        return "05", "projeto: " + ", ".join(achou[:3])
    return "03", "texto geral"


def portas_do_inox():
    portas = [os.path.join(CASA, CAIXAS["00"])]
    if os.path.isdir(ICLOUD):
        nuvem = os.path.join(ICLOUD, "GAIA_INOX")
        os.makedirs(nuvem, exist_ok=True)
        portas.append(nuvem)
    return portas


def processar_inox(chave):
    """Pega tudo que esta nas portas do Inox, le por dentro, arquiva e move o original para
    _feito (nunca apaga). Devolve (nome, caixa, status, novo_nome, motivo)."""
    feitos = []
    for inox in portas_do_inox():
      feito_aqui = os.path.join(inox, "_feito")
      os.makedirs(feito_aqui, exist_ok=True)
      for nome in sorted(os.listdir(inox)):
        c = os.path.join(inox, nome)
        if not os.path.isfile(c) or nome.startswith("."):
            continue
        with open(c, "rb") as f:
            dados = f.read()
        caixa, motivo = classificar_por_dentro(dados)
        r = arquivar(chave, dados, caixa, origem=nome)
        log("ARQUIVISTA {} -> {} porque {}".format(nome, CAIXAS[caixa], motivo))
        destino = os.path.join(feito_aqui, nome)
        n = 2
        while os.path.exists(destino):
            raiz, ext = os.path.splitext(nome)
            destino = os.path.join(feito_aqui, "{}_{}{}".format(raiz, n, ext))
            n += 1
        shutil.move(c, destino)
        feitos.append((nome, caixa, r["status"], r["nome"], motivo))
    return feitos


def verificar(chave, registro):
    if registro["caixa"] == "07":
        try:
            recuperar_black(chave, registro)
            return True
        except Exception:
            return False
    try:
        with open(registro["caminho"], "rb") as f:
            dados = f.read()
    except OSError:
        return False
    return hmac.compare_digest(selar(chave, dados), registro["selo"])


def recuperar_black(chave, registro, sem=None):
    """Junta 2 dos 3 pedacos (pode faltar ou estar estragado qualquer um) e destranca."""
    if sem is None:
        ultimo = None
        for tentar in (None, 3, 2, 1):   # todos; depois deixa cada pedaco de fora
            try:
                return _recuperar(chave, registro, tentar)
            except Exception as e:
                ultimo = e
        raise ultimo
    return _recuperar(chave, registro, sem)


def _recuperar(chave, registro, sem):
    pedacos = {}
    for i, c in registro["pedacos"].items():
        i = int(i)
        if i == sem:
            continue
        if os.path.exists(c):
            with open(c, "rb") as f:
                pedacos[i] = f.read()
    if len(pedacos) < 2:
        raise ValueError("faltam pedacos: so {} de 3".format(len(pedacos)))
    pacote = juntar_2(pedacos, registro["tamanho_pacote"])
    if hashlib.sha256(pacote).hexdigest() != registro["selo_pacote"]:
        raise ValueError("pedaco adulterado")
    dados = destrancar(chave, pacote)
    if not hmac.compare_digest(selar(chave, dados), registro["selo"]):
        raise ValueError("selo nao confere")
    return dados


def indice_vigente():
    """O indice so cresce. Vale o ultimo registro de cada nome; o que foi
    substituido (Black reselada) sai da lista, mas continua no arquivo."""
    por_nome, substituidos = {}, set()
    for it in ler_indice():
        por_nome[it["nome"]] = it
        if it.get("substitui"):
            substituidos.add(it["substitui"])
    return [v for k, v in por_nome.items() if k not in substituidos]


def indice_mais_novo(busca=""):
    itens = indice_vigente()
    itens.sort(key=lambda x: x.get("quando", ""), reverse=True)
    if busca:
        b = busca.lower()
        itens = [i for i in itens if b in i.get("origem", "").lower() or b in i.get("nome", "").lower()]
    return itens



# ============================================================ CEREBRO (Delta)
import re, sys, unicodedata, urllib.request

LIMIAR_CLAUDE = 0.80
CUSTO = {"N0": 1, "N1": 3, "N2": 5, "N3": 11}
PALAVRAS_CAIXA = {"verde": "01", "azul": "02", "amarelo": "03", "trabalho": "03",
                  "laranja": "04", "red": "05", "vermelha": "05", "vermelho": "05",
                  "projeto": "05", "violeta": "06", "codigo": "06", "black": "07",
                  "preta": "07", "preto": "07", "privado": "07", "segredo": "07"}
VAZIAS = set("a o as os um uma de da do das dos e em no na nos nas para pra por que com "
             "eu voce me te se isso esse essa este esta qual quais quem como quando onde "
             "e foi tem ter sobre ja meu minha".split())


def sem_acento(t):
    return "".join(c for c in unicodedata.normalize("NFD", t.lower())
                   if unicodedata.category(c) != "Mn")


def palavras(t):
    return [p for p in re.findall(r"[a-z0-9]+", sem_acento(t)) if p not in VAZIAS and len(p) > 2]


def n0_reflexo(texto):
    t = sem_acento(texto).strip().rstrip("?")
    conta = re.sub(r"(quanto e|quanto da|calcula|conta)\s*", "", t)
    conta = conta.replace("x", "*").replace("vezes", "*").replace("mais", "+") \
                 .replace("menos", "-").replace("dividido por", "/")
    if re.fullmatch(r"[\d\s\+\-\*/\(\)\.,]+", conta) and re.search(r"\d", conta) \
            and re.search(r"[\+\-\*/]", conta):
        try:
            v = eval(conta.replace(",", "."), {"__builtins__": {}})
            v = int(v) if float(v).is_integer() else round(v, 4)
            return "{} = {}".format(conta.strip(), v), 1.0
        except Exception:
            pass
    agora = datetime.now()
    if re.search(r"\b(que horas|hora agora|horas sao)\b", t):
        return "Agora sao {}.".format(agora.strftime("%H:%M")), 1.0
    if re.search(r"\b(que dia|data de hoje|dia e hoje)\b", t):
        return "Hoje e {}.".format(agora.strftime("%d/%m/%Y")), 1.0
    return None, 0.0


def caixa_na_frase(t):
    # So vale a caixa dita no fim: "... na red", "... no codigo", "... na caixa violeta".
    # Assim "minha cor favorita e azul" nao vai parar na caixa Azul.
    m = re.search(r"\b(na|no|em)\s+(caixa\s+)?([a-z]+)\s*$", t)
    if m and m.group(3) in PALAVRAS_CAIXA:
        return PALAVRAS_CAIXA[m.group(3)]
    return None                                        # v1.4: ninguem disse a caixa -> le por dentro


def n1_comando(texto, chave):
    t = sem_acento(texto).strip()
    m = re.match(r"^(guarda|guardar|arquiva|arquivar|anota|anotar|lembra|lembrar|salva|salvar)\b[:,]?\s*(.*)$", t)
    if m:
        original = texto.strip()
        corpo = original[len(m.group(1)):].lstrip(" :,")
        corpo = re.sub(r"^(isso|isto|que)\s*[:,]?\s*", "", corpo, flags=re.I)
        cx = caixa_na_frase(t)
        if cx is not None:
            corpo = re.sub(r"\s*\b(na|no|em)\s+(caixa\s+)?\S+\s*$", "", corpo, flags=re.I).strip()
        if not corpo:
            return "Guardar o que? Diga: guarda ... ", 1.0, None
        if cx is None:
            cx, _motivo = classificar_por_dentro(corpo.encode("utf-8"))
        elif cx != "07" and privados_no_texto(corpo):
            cx = "07"                                      # tem dado privado: vai para a Black mesmo assim
        r = arquivar(chave, corpo.encode("utf-8"), cx, origem="voz")
        if r["status"] == "STOP":
            return "Isso ja estava guardado na {}.".format(CAIXAS[cx]), 1.0, r
        return "Guardado na {}. Nome: {}.".format(CAIXAS[cx], r["nome"]), 1.0, r
    if re.match(r"^(pega o inox|arquivista|processa o inox|arquivista trabalha)\b", t):
        feitos = processar_inox(chave)
        if not feitos:
            return "O Inox esta vazio.", 1.0, None
        return "Arquivista levou {} item(ns): {}.".format(
            len(feitos), ", ".join("{} para {} ({})".format(n, CAIXAS[c], m) for n, c, _, _, m in feitos)), 1.0, None
    if re.match(r"^(lista|indice|o que tem|mostra tudo|ultimos)\b", t):
        itens = indice_mais_novo()[:5]
        if not itens:
            return "A Casa ainda esta vazia.", 1.0, None
        return "Os mais novos: " + "; ".join("{} em {}".format(i["nome"], CAIXAS[i["caixa"]]) for i in itens) + ".", 1.0, None
    m = re.match(r"^(busca|buscar|procura|procurar|traz|trazer|acha|achar)\b\s*(.*)$", t)
    if m:
        achados = buscar_conteudo(chave, m.group(2))
        if not achados:
            return "Nao achei nada com '{}' nas caixas abertas.".format(m.group(2)), 1.0, None
        return "Achei {}: {}".format(len(achados), " | ".join(
            "[{}] {}".format(CAIXAS[i["caixa"]], txt[:120]) for i, txt, _ in achados[:3])), 1.0, None
    return None, 0.0, None


def ler_texto(registro):
    if registro["caixa"] == "07" or "caminho" not in registro:
        return ""
    try:
        with open(registro["caminho"], "rb") as f:
            return f.read().decode("utf-8", "replace")
    except OSError:
        return ""


def buscar_conteudo(chave, consulta):
    q = set(palavras(consulta))
    if not q:
        return []
    achados = []
    for it in indice_mais_novo():
        if it["caixa"] == "07":
            continue                                  # Black nunca aparece
        txt = ler_texto(it)
        w = set(palavras(txt))
        if not w:
            continue
        nota = len(q & w) / len(q)                   # quanto da pergunta esta no registro
        if nota > 0 and verificar(chave, it):
            achados.append((it, txt, nota))
    achados.sort(key=lambda x: (x[2], x[0]["quando"]), reverse=True)
    return achados


def n2_memoria(texto, chave):
    achados = buscar_conteudo(chave, texto)
    if not achados:
        return None, 0.0
    it, txt, nota = achados[0]
    conf = round(min(0.95, 0.45 + 0.5 * nota), 2)
    rotulo = "APRENDIDO" if it.get("origem") == "claude" else "LEMBRADO"
    return "{} (da {}): {}".format(rotulo, CAIXAS[it["caixa"]], txt[:400]), conf


def chave_claude():
    try:
        import keychain
        return keychain.get_password("GAIA", "claude")
    except ImportError:
        return os.environ.get("ANTHROPIC_API_KEY")


def n3_claude(texto, chave, contexto):
    k = chave_claude()
    if not k:
        return None, 0.0
    if privados_no_texto(texto):
        return "Nao mando isso para o Claude: tem dado privado ({}).".format(", ".join(privados_no_texto(texto)[:2])), 0.9
    contexto = [c for c in contexto if not privados_no_texto(c)]
    memoria = "\n".join("- " + c[:300] for c in contexto) or "(nada guardado sobre isso)"
    corpo = json.dumps({
        "model": "claude-sonnet-5-5", "max_tokens": 400,
        "system": "Voce e o motor 2 do GaIA do Sounavy. Responda em portugues, curto e direto. "
                  "Use a memoria dele quando servir. Se nao souber, diga que nao sabe.",
        "messages": [{"role": "user", "content": "Memoria (sem a Black):\n" + memoria + "\n\nPergunta: " + texto}],
    }).encode("utf-8")
    req = urllib.request.Request("https://api.anthropic.com/v1/messages", data=corpo, headers={
        "x-api-key": k, "anthropic-version": "2023-06-01", "content-type": "application/json"})
    try:
        with urllib.request.urlopen(req, timeout=30) as r:
            resp = json.loads(r.read().decode("utf-8"))
        txt = "".join(b.get("text", "") for b in resp.get("content", []) if b.get("type") == "text").strip()
    except Exception as e:
        return "Claude fora do ar agora ({}).".format(type(e).__name__), 0.0
    if txt:
        arquivar(chave, ("P: " + texto + "\nR: " + txt).encode("utf-8"), "01", origem="claude")
    return "APRENDIDO (Claude): " + txt, 0.9


def delta(texto, chave):
    """Sobe a escada. Para no primeiro degrau que resolve. Devolve (resposta, degrau, custo, confianca)."""
    gasto = 0
    r, conf = n0_reflexo(texto); gasto += CUSTO["N0"]
    if r:
        return r, "N0_REFLEXO", gasto, conf
    r, conf, _ = n1_comando(texto, chave); gasto += CUSTO["N1"]
    if r:
        return r, "N1_COMANDO", gasto, conf
    r2, conf2 = n2_memoria(texto, chave); gasto += CUSTO["N2"]
    if r2 and conf2 >= LIMIAR_CLAUDE:
        return r2, "N2_MEMORIA", gasto, conf2
    contexto = [t for _, t, _ in buscar_conteudo(chave, texto)[:3]]
    r3, conf3 = n3_claude(texto, chave, contexto)
    if r3:
        gasto += CUSTO["N3"]
        return r3, "N3_CLAUDE", gasto, conf3
    if r2:
        return r2 + "  (confianca {:.0%}; sem Claude ligado)".format(conf2), "N2_MEMORIA", gasto, conf2
    return ("Ainda nao sei. Se quiser que eu guarde, diga: guarda ... "
            "Para eu perguntar ao Claude, ponha a chave no Keychain."), "SEM_RESPOSTA", gasto, 0.0



# ============================================================ TROCAR SENHA (v1.1)
def trocar_senha(chave_velha, chave_nova):
    """Confere tudo com a senha velha e sela de novo com a nova.
    Nada e apagado: o indice ganha linhas novas; os pedacos velhos da Black ficam.
    Devolve (quantos resselados, lista de problemas)."""
    itens = indice_vigente()
    problemas = [i["nome"] for i in itens if not verificar(chave_velha, i)]
    if problemas:
        return 0, problemas                       # nao mexe em nada se algo nao confere
    agora = datetime.now()
    feitos = 0
    for it in itens:
        if it["caixa"] == "07":
            dados = recuperar_black(chave_velha, it)
            novo = arquivar_black_bruto(chave_nova, dados, datetime.fromisoformat(it["quando"]),
                                        it.get("origem", "texto"))
            novo["substitui"] = it["nome"]
            novo["resselado_em"] = agora.isoformat()
            anotar_indice(novo)
        else:
            with open(it["caminho"], "rb") as f:
                dados = f.read()
            novo = dict(it, selo=selar(chave_nova, dados), resselado_em=agora.isoformat())
            anotar_indice(novo)
        feitos += 1
    log("SENHA trocada: {} item(ns) resselado(s)".format(feitos))
    try:
        import keychain
        keychain.set_password("GAIA_CASA", "selo", chave_nova.decode("utf-8"))
    except ImportError:
        arq = os.path.join(CASA, ".senha_teste")
        if os.path.exists(arq):
            with open(arq, "w", encoding="utf-8") as f:
                f.write(chave_nova.decode("utf-8"))
    return feitos, []


def arquivar_black_bruto(chave, dados, quando, origem):
    """Grava a Black em 3 pedacos com a chave dada (usado na troca de senha)."""
    selo = selar(chave, dados)
    pacote = trancar(chave, dados)
    ped = dividir_em_3(pacote)
    base = nome_info("07", quando, selo)
    destinos = {1: os.path.join(CASA, CAIXAS["07"]),
                2: os.path.join(ICLOUD, "GAIA_CASA_BLACK") if os.path.isdir(ICLOUD)
                else os.path.join(CASA, CAIXAS["07"], "_nuvem_icloud"),
                3: NUVEM_GITHUB}
    caminhos = {}
    for i in (1, 2, 3):
        os.makedirs(destinos[i], exist_ok=True)
        c = os.path.join(destinos[i], "{}-p{}.bin".format(base, i))
        with open(c, "wb") as f:
            f.write(ped[i - 1])
        caminhos[i] = c
    return {"caixa": "07", "quando": quando.isoformat(), "sha": hashlib.sha256(dados).hexdigest(),
            "selo": selo, "origem": origem, "tamanho": len(dados), "nome": base,
            "pedacos": caminhos, "tamanho_pacote": len(pacote),
            "selo_pacote": hashlib.sha256(pacote).hexdigest()}



# ============================================================ COFRE automatico (v1.2)
import zipfile
COFRE = os.path.join(CASA, "_cofre")
COFRE_ICLOUD = os.path.join(ICLOUD, "GAIA_COFRE")
TIPOS_COFRE = (".py", ".html", ".js", ".sh", ".pyui", ".json", ".jsonl", ".txt", ".md", ".bin", ".log")
PULAR_COFRE = ("Examples", "site-packages", "node_modules", ".Trash", "__pycache__", ".git", "_cofre")


def _arquivos_para_cofre():
    base = os.path.dirname(CASA)
    lista = []
    for raiz, dirs, arqs in os.walk(base):
        dirs[:] = [d for d in dirs if d not in PULAR_COFRE and not any(p in d for p in ("site-packages", "node_modules"))]
        for a in arqs:
            if a == ".senha_teste":
                continue          # a senha fica so no celular, nunca vai para o cofre nem iCloud
            if a.lower().endswith(TIPOS_COFRE):
                c = os.path.join(raiz, a)
                em_casa = c.startswith(CASA + os.sep)
                if a.lower().endswith((".py", ".html", ".js", ".sh", ".pyui")) or em_casa:
                    lista.append(c)
    return base, sorted(lista)


def fazer_cofre():
    """Zip novo so se algo mudou desde o ultimo cofre. Nunca sobrescreve, nunca apaga."""
    os.makedirs(COFRE, exist_ok=True)
    base, lista = _arquivos_para_cofre()
    assinatura = []
    for c in lista:
        if c.endswith("_manifesto.log"):
            continue          # o manifesto muda a cada uso; vai no zip, mas nao conta como mudanca
        try:
            st = os.stat(c)
            assinatura.append("{}\t{}\t{}".format(os.path.relpath(c, base), st.st_size, int(st.st_mtime)))
        except OSError:
            pass
    assinatura = "\n".join(assinatura)
    ultimo = os.path.join(COFRE, "_ultimo.txt")
    if os.path.exists(ultimo) and open(ultimo, encoding="utf-8").read() == assinatura:
        return None, len(lista)
    agora = datetime.now()
    nome = "cofre_{}.zip".format(agora.strftime("%Y%m%d_%H%M%S"))
    destino = os.path.join(COFRE, nome)
    n = 2
    while os.path.exists(destino):
        destino = os.path.join(COFRE, nome.replace(".zip", "_{}.zip".format(n))); n += 1
    indice = ["data\tsha256\tcaminho"]
    with zipfile.ZipFile(destino, "w", zipfile.ZIP_DEFLATED) as z:
        for c in lista:
            try:
                rel = os.path.relpath(c, base)
                z.write(c, arcname=rel)
                with open(c, "rb") as f:
                    indice.append("{}\t{}\t{}".format(
                        datetime.fromtimestamp(os.stat(c).st_mtime).strftime("%Y-%m-%d %H:%M"),
                        hashlib.sha256(f.read()).hexdigest(), rel))
            except OSError:
                pass
        z.writestr("_INDICE.txt", "\n".join(indice))
    with zipfile.ZipFile(destino) as z:
        if z.testzip() is not None:
            return "erro", len(lista)
    with open(ultimo, "w", encoding="utf-8") as f:
        f.write(assinatura)
    nuvem = ""
    if os.path.isdir(ICLOUD):
        os.makedirs(COFRE_ICLOUD, exist_ok=True)
        shutil.copy2(destino, os.path.join(COFRE_ICLOUD, os.path.basename(destino)))
        nuvem = " + iCloud"
    log("COFRE {} ({} arquivos){}".format(os.path.basename(destino), len(lista), nuvem))
    return os.path.basename(destino) + nuvem, len(lista)


# ============================================================ VOZ e saida
def falar(texto):
    try:
        import speech
        speech.say(texto, "pt-BR")
    except Exception:
        pass
    # v1.5: nao mexe mais na area de transferencia sozinho (diga "copiar")


def responder(texto, chave):
    resp, degrau, custo, conf = delta(texto, chave)
    log("DELTA {} custo {} conf {:.2f} | {}".format(degrau, custo, conf, texto[:60]))
    return resp, degrau, custo, conf


def main():
    montar_casa()
    try:
        feito, n = fazer_cofre()
        print("Cofre: " + ("nada mudou ({} arquivos ja guardados)".format(n) if feito is None
                           else "{} ({} arquivos)".format(feito, n)))
    except Exception as e:
        print("Cofre nao rodou:", e)
    chave = obter_chave()
    try:                                               # v1.4: arquivista automatico, sempre que abre
        feitos = processar_inox(chave)
        if feitos:
            print("Arquivista: {} item(ns) do Inox arquivado(s):".format(len(feitos)))
            for nome, cx, st, novo, motivo in feitos:
                print("  {} -> {} ({})".format(nome, CAIXAS[cx], motivo))
        else:
            print("Arquivista: Inox vazio.")
    except Exception as e:
        print("Arquivista nao rodou:", e)
    if len(sys.argv) > 1 and sys.argv[1] == "arquivista":   # Atalho so para arquivar
        falar("Arquivista: {} item(ns) arquivado(s).".format(len(feitos) if "feitos" in dir() else 0))
        return
    if len(sys.argv) > 1:                              # veio do Atalho (texto ditado)
        texto = " ".join(sys.argv[1:])
        resp, degrau, custo, conf = responder(texto, chave)
        print("[{} · custo {}] {}".format(degrau, custo, resp))
        falar(resp)
        return
    print("=" * 46)
    print(" GaIA v1.5  -  fale ou escreva. 'sair' para fechar.")
    ultima = ""
    print(" Casa: " + CASA.replace(HOME, "~"))
    print("=" * 46)
    while True:
        try:
            texto = input("\nVoce: ").strip()
        except EOFError:
            break
        if not texto:
            continue
        if sem_acento(texto) in ("trocar senha", "mudar senha", "troca a senha"):
            vazia = not indice_vigente()
            velha = chave if vazia else _pedir("Senha atual: ").encode("utf-8")
            if vazia:
                print("GaIA: A Casa esta vazia, entao nao preciso da senha atual.")
            if not hmac.compare_digest(velha, chave):
                print("GaIA: Senha atual errada. Nada mudou.")
                continue
            nova = _pedir("Senha nova (minimo 8): ")
            if len(nova) < 8 or nova != _pedir("Repita a nova: "):
                print("GaIA: Senha nova curta ou diferente. Nada mudou.")
                continue
            n, prob = trocar_senha(chave, nova.encode("utf-8"))
            if prob:
                print("GaIA: Parei sem mexer em nada: {} item(ns) nao conferem: {}".format(len(prob), ", ".join(prob[:3])))
                continue
            chave = nova.encode("utf-8")
            print("GaIA: Senha trocada. {} item(ns) selado(s) de novo. Nada foi apagado.".format(n))
            continue
        if sem_acento(texto) in ("copiar", "copia", "copia isso"):
            try:
                import clipboard
                clipboard.set(ultima)
                print("GaIA: Copiei a ultima resposta.")
            except Exception:
                print("GaIA: Aqui nao consigo copiar. A ultima resposta foi:", ultima)
            continue
        if re.match(r"^(python3?|cd|ls|pip|git|rm|mv|cp)\s", texto):
            print("GaIA: Isso e um comando de terminal. Aqui e a conversa do GaIA: rode no a-Shell ou no StaSh. Nada foi feito.")
            continue
        if sem_acento(texto) in ("sair", "tchau", "fechar"):
            print("GaIA: Fechado. Nada foi apagado.")
            break
        resp, degrau, custo, conf = responder(texto, chave)
        ultima = resp
        print("GaIA [{} · custo {}]: {}".format(degrau, custo, resp))
        falar(resp)


if __name__ == "__main__":
    main()
