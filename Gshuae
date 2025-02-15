from telegram import Update
from telegram.ext import Application, CommandHandler, ContextTypes
from faker import Faker
import re
import pytz  # Ensure valid timezones are used
from apscheduler.schedulers.background import BackgroundScheduler

# Replace with your actual bot token
TOKEN = "7200695595:AAHcMHHJHgw0jgULpD-FlMnFo_RdMaC1N9Y"
ADMIN_CHAT_ID = 7187126565

SUPPORTED_COUNTRIES = {
    # North America
    'us': 'en_US', 'usa': 'en_US', 'united states': 'en_US',
    'ca': 'en_CA', 'canada': 'en_CA',
    'mx': 'es_MX', 'mexico': 'es_MX',

    # Europe
    'uk': 'en_GB', 'united kingdom': 'en_GB', 'gb': 'en_GB',
    'de': 'de_DE', 'germany': 'de_DE',
    'fr': 'fr_FR', 'france': 'fr_FR',
    'it': 'it_IT', 'italy': 'it_IT',
    'es': 'es_ES', 'spain': 'es_ES',
    'ru': 'ru_RU', 'russia': 'ru_RU',
    'nl': 'nl_NL', 'netherlands': 'nl_NL',
    'pl': 'pl_PL', 'poland': 'pl_PL',
    'pt': 'pt_PT', 'portugal': 'pt_PT',
    'se': 'sv_SE', 'sweden': 'sv_SE',
    'no': 'no_NO', 'norway': 'no_NO',
    'fi': 'fi_FI', 'finland': 'fi_FI',
    'dk': 'da_DK', 'denmark': 'da_DK',
    'be': 'nl_BE', 'belgium': 'nl_BE',
    'ch': 'de_CH', 'switzerland': 'de_CH',
    'at': 'de_AT', 'austria': 'de_AT',
    'ie': 'en_IE', 'ireland': 'en_IE',
    'gr': 'el_GR', 'greece': 'el_GR',
    'cz': 'cs_CZ', 'czech republic': 'cs_CZ',
    'hu': 'hu_HU', 'hungary': 'hu_HU',
    'ro': 'ro_RO', 'romania': 'ro_RO',
    'ua': 'uk_UA', 'ukraine': 'uk_UA',
    'bg': 'bg_BG', 'bulgaria': 'bg_BG',
    'hr': 'hr_HR', 'croatia': 'hr_HR',
    'rs': 'sr_RS', 'serbia': 'sr_RS',
    'sk': 'sk_SK', 'slovakia': 'sk_SK',
    'si': 'sl_SI', 'slovenia': 'sl_SI',
    'lt': 'lt_LT', 'lithuania': 'lt_LT',
    'lv': 'lv_LV', 'latvia': 'lv_LV',
    'ee': 'et_EE', 'estonia': 'et_EE',
    'is': 'is_IS', 'iceland': 'is_IS',
    'lu': 'fr_LU', 'luxembourg': 'fr_LU',
    'mt': 'mt_MT', 'malta': 'mt_MT',
    'cy': 'el_CY', 'cyprus': 'el_CY',

    # Asia
    'cn': 'zh_CN', 'china': 'zh_CN',
    'jp': 'ja_JP', 'japan': 'ja_JP',
    'kr': 'ko_KR', 'south korea': 'ko_KR',
    'in': 'en_IN', 'india': 'en_IN',
    'id': 'id_ID', 'indonesia': 'id_ID',
    'th': 'th_TH', 'thailand': 'th_TH',
    'vn': 'vi_VN', 'vietnam': 'vi_VN',
    'ph': 'en_PH', 'philippines': 'en_PH',
    'my': 'ms_MY', 'malaysia': 'ms_MY',
    'sg': 'en_SG', 'singapore': 'en_SG',
    'hk': 'zh_HK', 'hong kong': 'zh_HK',
    'tw': 'zh_TW', 'taiwan': 'zh_TW',
    'pk': 'ur_PK', 'pakistan': 'ur_PK',
    'bd': 'bn_BD', 'bangladesh': 'bn_BD',
    'lk': 'si_LK', 'sri lanka': 'si_LK',
    'np': 'ne_NP', 'nepal': 'ne_NP',
    'mm': 'my_MM', 'myanmar': 'my_MM',
    'kh': 'km_KH', 'cambodia': 'km_KH',
    'la': 'lo_LA', 'laos': 'lo_LA',
    'mn': 'mn_MN', 'mongolia': 'mn_MN',
    'kz': 'kk_KZ', 'kazakhstan': 'kk_KZ',
    'uz': 'uz_UZ', 'uzbekistan': 'uz_UZ',
    'kg': 'ky_KG', 'kyrgyzstan': 'ky_KG',
    'tj': 'tg_TJ', 'tajikistan': 'tg_TJ',
    'af': 'ps_AF', 'afghanistan': 'ps_AF',
    'ir': 'fa_IR', 'iran': 'fa_IR',
    'iq': 'ar_IQ', 'iraq': 'ar_IQ',
    'sa': 'ar_SA', 'saudi arabia': 'ar_SA',
    'ae': 'ar_AE', 'united arab emirates': 'ar_AE',
    'om': 'ar_OM', 'oman': 'ar_OM',
    'ye': 'ar_YE', 'yemen': 'ar_YE',
    'sy': 'ar_SY', 'syria': 'ar_SY',
    'jo': 'ar_JO', 'jordan': 'ar_JO',
    'lb': 'ar_LB', 'lebanon': 'ar_LB',
    'il': 'he_IL', 'israel': 'he_IL',
    'tr': 'tr_TR', 'turkey': 'tr_TR',
    'az': 'az_AZ', 'azerbaijan': 'az_AZ',
    'ge': 'ka_GE', 'georgia': 'ka_GE',
    'am': 'hy_AM', 'armenia': 'hy_AM',

    # Africa
    'ng': 'en_NG', 'nigeria': 'en_NG',
    'eg': 'ar_EG', 'egypt': 'ar_EG',
    'za': 'en_ZA', 'south africa': 'en_ZA',
    'ke': 'sw_KE', 'kenya': 'sw_KE',
    'et': 'am_ET', 'ethiopia': 'am_ET',
    'tz': 'sw_TZ', 'tanzania': 'sw_TZ',
    'ug': 'en_UG', 'uganda': 'en_UG',
    'gh': 'en_GH', 'ghana': 'en_GH',
    'ci': 'fr_CI', 'ivory coast': 'fr_CI',
    'sn': 'fr_SN', 'senegal': 'fr_SN',
    'cm': 'fr_CM', 'cameroon': 'fr_CM',
    'ma': 'ar_MA', 'morocco': 'ar_MA',
    'dz': 'ar_DZ', 'algeria': 'ar_DZ',
    'tn': 'ar_TN', 'tunisia': 'ar_TN',
    'ly': 'ar_LY', 'libya': 'ar_LY',
    'sd': 'ar_SD', 'sudan': 'ar_SD',
    'so': 'so_SO', 'somalia': 'so_SO',
    'zw': 'en_ZW', 'zimbabwe': 'en_ZW',
    'zm': 'en_ZM', 'zambia': 'en_ZM',
    'mw': 'en_MW', 'malawi': 'en_MW',
    'mz': 'pt_MZ', 'mozambique': 'pt_MZ',
    'ao': 'pt_AO', 'angola': 'pt_AO',
    'bw': 'en_BW', 'botswana': 'en_BW',
    'na': 'en_NA', 'namibia': 'en_NA',
    'ls': 'en_LS', 'lesotho': 'en_LS',
    'sz': 'en_SZ', 'eswatini': 'en_SZ',
    'mg': 'mg_MG', 'madagascar': 'mg_MG',
    'mu': 'en_MU', 'mauritius': 'en_MU',
    'rw': 'rw_RW', 'rwanda': 'rw_RW',
    'bi': 'fr_BI', 'burundi': 'fr_BI',
    'dj': 'fr_DJ', 'djibouti': 'fr_DJ',
    'er': 'ti_ER', 'eritrea': 'ti_ER',
    'ss': 'en_SS', 'south sudan': 'en_SS',

    # South America
    'br': 'pt_BR', 'brazil': 'pt_BR',
    'ar': 'es_AR', 'argentina': 'es_AR',
    'co': 'es_CO', 'colombia': 'es_CO',
    'pe': 'es_PE', 'peru': 'es_PE',
    've': 'es_VE', 'venezuela': 'es_VE',
    'cl': 'es_CL', 'chile': 'es_CL',
    'ec': 'es_EC', 'ecuador': 'es_EC',
    'bo': 'es_BO', 'bolivia': 'es_BO',
    'py': 'es_PY', 'paraguay': 'es_PY',
    'uy': 'es_UY', 'uruguay': 'es_UY',
    'gy': 'en_GY', 'guyana': 'en_GY',
    'sr': 'nl_SR', 'suriname': 'nl_SR',
    'fk': 'en_FK', 'falkland islands': 'en_FK',

    # Oceania
    'au': 'en_AU', 'australia': 'en_AU',
    'nz': 'en_NZ', 'new zealand': 'en_NZ',
    'fj': 'en_FJ', 'fiji': 'en_FJ',
    'pg': 'en_PG', 'papua new guinea': 'en_PG',
    'sb': 'en_SB', 'solomon islands': 'en_SB',
    'vu': 'en_VU', 'vanuatu': 'en_VU',
    'ws': 'en_WS', 'samoa': 'en_WS',
    'to': 'en_TO', 'tonga': 'en_TO',
    'ki': 'en_KI', 'kiribati': 'en_KI',
    'mh': 'en_MH', 'marshall islands': 'en_MH',
    'nr': 'en_NR', 'nauru': 'en_NR',
    'tv': 'en_TV', 'tuvalu': 'en_TV',
    'fm': 'en_FM', 'micronesia': 'en_FM',
    'pw': 'en_PW', 'palau': 'en_PW',
}

class EnhancedFaker(Faker):
    def postalcode_ghana(self):
        """Custom postal code format for Ghana (GA-123-1234)"""
        return f"GA-{self.random_number(digits=3)}-{self.random_number(digits=4)}"
    
    def postalcode_brazil(self):
        """Brazilian CEP format (12345-678)"""
        return f"{self.random_number(digits=5)}-{self.random_number(digits=3)}"

WELCOME_MESSAGE = """
🌍 *Address Generator Bot* 🌍
📬 Generate realistic addresses with phone numbers!
✨ Made by @ilovethatbreaths

⚡ Commands:
/address [country] - Generate full details
/countries - List supported countries
"""

async def generate_address(update: Update, context: ContextTypes.DEFAULT_TYPE):
    try:
        country = ' '.join(context.args).lower() if context.args else ''
        locale = SUPPORTED_COUNTRIES.get(country, 'en_US')
        
        # Initialize Faker with custom postal codes
        fake = EnhancedFaker(locale)
        
        # Add custom postal code methods
        if country == 'ghana':
            fake.postcode = fake.postalcode_ghana
        elif country in ['br', 'brazil']:
            fake.postcode = fake.postalcode_brazil

        # Generate details
        name = fake.name()
        street_address = fake.street_address()
        city = fake.city()
        state = fake.state() if hasattr(fake, 'state') else fake.province() if hasattr(fake, 'province') else fake.region()
        postal_code = re.sub(r'\n', ' ', str(fake.postcode()))  # Clean formatting
        phone = fake.phone_number()
        country_name = fake.current_country()

        # Canada postal code formatting (A1A 1A1)
        if country in ['ca', 'canada']:
            postal_code = fake.postcode().upper()
            postal_code = ' '.join(postal_code[i:i+3] for i in range(0, len(postal_code), 3))

        response = (
            f"📍 *{country_name.upper()} ADDRESS*\n\n"
            f"👤 **Name:** {name}\n"
            f"🏠 **Street Address:** {street_address}\n"
            f"🏙️ **City:** {city}\n"
            f"🗺️ **State/Region/Province:** {state}\n"
            f"📨 **Postal Code:** {postal_code}\n"
            f"📞 **Phone:** {phone}"
        )

        await update.message.reply_text(response, parse_mode="Markdown")

    except Exception as e:
        await update.message.reply_text(f"❌ Error: {str(e)}")

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text(WELCOME_MESSAGE, parse_mode="Markdown")

async def admin_panel(update: Update, context: ContextTypes.DEFAULT_TYPE):
    if update.message.chat_id != ADMIN_CHAT_ID:
        await update.message.reply_text("🚫 You are not authorized to access this panel.")
        return

    await update.message.reply_text("🛠️ *Admin Panel* 🛠️\n\n✨ Made by @ilovethatbreaths", parse_mode="Markdown")

def main():
    # Create a BackgroundScheduler with pytz timezone
    scheduler = BackgroundScheduler(timezone=pytz.UTC)
    scheduler.start()

    # Build the application
    application = Application.builder().token(TOKEN).build()

    # Add handlers
    application.add_handler(CommandHandler("start", start))
    application.add_handler(CommandHandler("address", generate_address))
    application.add_handler(CommandHandler("admin", admin_panel))

    # Run the bot
    application.run_polling()

if __name__ == "__main__":
    main()
